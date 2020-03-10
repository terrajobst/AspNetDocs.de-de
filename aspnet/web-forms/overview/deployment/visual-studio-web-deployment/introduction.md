---
uid: web-forms/overview/deployment/visual-studio-web-deployment/introduction
title: 'ASP.net-Webbereitstellung mithilfe von Visual Studio: Einführung | Microsoft-Dokumentation'
author: tdykstra
description: In dieser tutorialreihe erfahren Sie, wie Sie eine ASP.NET-Webanwendung mithilfe von V... zum Azure App Service von Web-Apps oder eines Drittanbieter-hostinganbieters bereitstellen (veröffentlichen).
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 24ad086d-865e-433c-9ac9-05f1a553da16
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/introduction
msc.type: authoredcontent
ms.openlocfilehash: 96dd31d949633e001fc595621bedbf74e98000fc
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78521757"
---
# <a name="aspnet-web-deployment-using-visual-studio-introduction"></a>ASP.net-Webbereitstellung mithilfe von Visual Studio: Einführung

von [Tom Dykstra](https://github.com/tdykstra)

[Starter Projekt herunterladen](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> In dieser tutorialreihe erfahren Sie, wie Sie eine ASP.NET-Webanwendung mithilfe von Visual Studio 2012 mit dem Azure SDK für .net für die Azure App Service von Web-Apps oder eines Drittanbieter-hostinganbieters bereitstellen (veröffentlichen). Die meisten der Prozeduren sind für Visual Studio 2013 ähnlich.
> 
> Sie entwickeln eine Webanwendung, um Sie für Personen über das Internet verfügbar zu machen. Webprogrammier Lernprogramme werden jedoch in der Regel direkt beendet, nachdem Sie Ihnen gezeigt haben, wie Sie auf Ihrem Entwicklungs Computer arbeiten. Diese Reihe von Tutorials beginnt, wenn die anderen unterscheiden: Sie haben eine Web-App erstellt, getestet und sind bereit für den Start. Ausblick In diesen Tutorials erfahren Sie, wie Sie zuerst in IIS auf Ihrem lokalen Entwicklungs Computer zum Testen und dann zu Azure oder einem Drittanbieter für die Staging-und Produktionsumgebung bereitstellen. Die Beispielanwendung, die Sie bereitstellen, ist ein Webanwendungs Projekt, das die Entity Framework, SQL Server und das ASP.NET-Mitgliedschaftssystem verwendet. Die Beispielanwendung verwendet ASP.net Web Forms, aber die gezeigten Prozeduren gelten auch für ASP.NET MVC und die Web-API.
> 
> In diesen Tutorials wird davon ausgegangen, dass Sie mit der Arbeit mit ASP.net in Visual Studio vertraut sind. Wenn Sie dies nicht tun, empfiehlt es sich, ein [einfaches ASP.net-Web Forms Tutorial](../../older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1.md) oder ein [einfaches ASP.NET MVC-Tutorial](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md)zu starten.
> 
> Wenn Sie Fragen haben, die nicht direkt mit dem Tutorial zusammenhängen, können Sie Sie im [ASP.net-Bereitstellungs Forum](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) oder in [StackOverflow](http://stackoverflow.com)veröffentlichen.
> 
> Dieser Inhalt ist auch als kostenloses e-book in [der TechNet-e-book-Galerie](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#ASPNETWebDeploymentusingVisualStudio)verfügbar.

## <a name="overview"></a>Übersicht

Diese Tutorials führen Sie durch die Bereitstellung einer ASP.NET-Webanwendung, die SQL Server Datenbanken umfasst. Sie werden zuerst in IIS auf Ihrem lokalen Entwicklungs Computer bereitstellen, um Sie zu testen, und dann zu Web-Apps in Azure App Service und Azure SQL-Datenbank für Staging und Produktion. Sie erfahren, wie Sie die Bereitstellung mithilfe von Visual Studio One-Click Publish durchzuführen, und Sie sehen, wie Sie die Bereitstellung über die Befehlszeile durchzuführen.

Die Anzahl der Tutorials macht den Bereitstellungs Prozess möglicherweise erschreckend. Tatsächlich sind die grundlegenden Prozeduren einfach. In realen Situationen müssen Sie jedoch häufig zusätzliche Bereitstellungs Aufgaben ausführen – z. b. das Festlegen von Ordner Berechtigungen auf dem Zielserver. Wir haben einige dieser zusätzlichen Aufgaben erläutert, in der Hoffnung, dass die Lernprogramme keine Informationen hinterlassen, die möglicherweise verhindern, dass eine echte Anwendung erfolgreich bereitgestellt wird.

Die Lernprogramme sind so konzipiert, dass Sie nacheinander ausgeführt werden, und jeder Teil basiert auf dem vorherigen Teil. Sie können Teile überspringen, die für Ihre Situation nicht relevant sind, aber Sie müssen die Verfahren möglicherweise in späteren Tutorials anpassen.

## <a name="intended-audience"></a>Zielpublikum

Die Tutorials richten sich an ASP.NET-Entwickler, die in Umgebungen arbeiten, in denen Folgendes gilt:

- Die Produktionsumgebung ist Azure App Service Web-Apps oder ein Drittanbieter-Hostinganbieter.
- Die Bereitstellung ist nicht auf einen Continuous Integration Prozess beschränkt, kann jedoch direkt in Visual Studio ausgeführt werden.

Die Bereitstellung aus der [Quell](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md) Code Verwaltung mithilfe eines [Continuous Delivery](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) Prozesses wird in diesen Tutorials nicht behandelt, mit Ausnahme eines Tutorials, in dem die Bereitstellung über die Befehlszeile veranschaulicht wird. Weitere Informationen zu Continuous Delivery finden Sie in den folgenden Ressourcen:

- [Continuous Integration und Continuous Delivery (entwickeln realer Cloud-apps mit Windows Azure)](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md)
- [Bereitstellen von Web-Apps in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-deploy/)
- Bereitstellen von [Webanwendungen in Unternehmens Szenarios](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md) (eine ältere Reihe von Tutorials, die für Visual Studio 2010 geschrieben wurden und dennoch nützliche Informationen für Unternehmensumgebungen haben.)

## <a name="using-a-third-party-hosting-provider"></a>Verwenden eines Drittanbieter-hostinganbieters

Die Lernprogramme führen Sie durch die Einrichtung eines Azure-Kontos und die Bereitstellung der Anwendung für Web-Apps in Azure App Service für Staging und Produktion. Sie können jedoch die gleichen grundlegenden Verfahren für die Bereitstellung für einen Drittanbieter-Hostinganbieter Ihrer Wahl verwenden. In den Lernprogrammen werden die in Azure einzigartigen Prozesse erläutert, und es wird erläutert, welche Unterschiede Sie bei einem Drittanbieter-Hostinganbieter erwarten können.

## <a name="deploying-web-app-projects"></a>Bereitstellen von Web App-Projekten

Die Beispielanwendung, die Sie für diese Tutorials herunterladen und bereitstellen, ist ein Visual Studio-Webanwendungs Projekt. Wenn Sie jedoch das aktuellste Web-Veröffentlichungs Update für Visual Studio installieren, können Sie dieselben Bereitstellungs Methoden und Tools für Web-App-Projekte verwenden.

## <a name="deploying-aspnet-mvc-projects"></a>Bereitstellen von ASP.NET MVC-Projekten

Bei der Beispielanwendung handelt es sich um ein ASP.net-Web Forms Projekt, aber alles, was Sie lernen, gilt auch für ASP.NET MVC. Ein Visual Studio-MVC-Projekt ist nur eine andere Form des Webanwendungs Projekts. Der einzige Unterschied besteht darin, dass Sie bei der Bereitstellung auf einem Hosting-Anbieter, der ASP.NET MVC oder Ihre Zielversion nicht unterstützt, sicherstellen müssen, dass das entsprechende nuget-Paket ([MVC 3](http://nuget.org/packages/AspNetMvc/3.0.20105.0), [MVC 4](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/4.0.30506.0)oder [MVC 5](http://www.nuget.org/packages/Microsoft.AspNet.Mvc)) in Ihrem Projekt installiert ist.

## <a name="programming-language"></a>Programmiersprache

Die Beispielanwendung verwendet C# , aber die Lernprogramme erfordern keine Kenntnisse C#von, und die von den Tutorials gezeigten Bereitstellungs Techniken sind nicht sprachspezifisch.

## <a name="database-deployment-methods"></a>Daten Bank Bereitstellungs Methoden

Es gibt drei Möglichkeiten, wie Sie eine SQL Server-Datenbank zusammen mit der Webbereitstellung in Visual Studio bereitstellen können:

- Entity Framework Code First-Migrationen
- Der dbdacfx-Web deploy-Anbieter
- Der dbfullsql-Web deploy-Anbieter

In diesem Tutorial verwenden Sie die ersten beiden Methoden. Der dbfullsql-Web deploy-Anbieter ist eine Legacy Methode, die nicht mehr empfohlen wird, außer für bestimmte Szenarien, z. b. die Migration von SQL Server Compact zu SQL Server.

Die in diesem Tutorial gezeigten Methoden gelten für SQL Server Datenbanken, nicht für SQL Server Compact. Weitere Informationen zum Bereitstellen einer SQL Server Compact-Datenbank finden Sie unter [Visual Studio-Webbereitstellung mit SQL Server Compact](../../older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md).

Für die in diesem Tutorial gezeigten Methoden ist es erforderlich, dass Sie die Web deploy Publish-Methode verwenden. Wenn Sie eine andere Veröffentlichungs Methode bevorzugen, z. b. FTP, Datei System oder FPSE, finden Sie weitere [Informationen unter Bereitstellen einer Datenbank getrennt von der Webanwendungs Bereitstellung](https://go.microsoft.com/fwlink/p/?LinkId=282413#databaseseparate) in der Webbereitstellungs-Inhalts Zuordnung für Visual Studio und ASP.net.

### <a name="entity-framework-code-first-migrations"></a>Entity Framework Code First-Migrationen

In der Version 4,3 von Entity Framework hat Microsoft Code First-Migrationen eingeführt. Code First-Migrationen automatisiert den Prozess der Erstellung inkrementeller Änderungen an einem Datenmodell und der Weitergabe dieser Änderungen an die Datenbank. In früheren Versionen von Code First lassen Sie das Entity Framework die Datenbank jedes Mal löschen und neu erstellen, wenn Sie das Datenmodell ändern. Dies ist kein Problem bei der Entwicklung, da Testdaten problemlos neu erstellt werden können. in der Produktionsumgebung möchten Sie jedoch in der Regel das Datenbankschema aktualisieren, ohne die Datenbank zu löschen. Die Migrations Funktion ermöglicht Code First, die Datenbank zu aktualisieren, ohne Sie zu löschen und neu zu erstellen. Sie können Code First automatisch entscheiden, wie Sie die erforderlichen Schema Änderungen vornehmen möchten, oder Sie können Code schreiben, mit dem die Änderungen angepasst werden. Eine Einführung in Code First-Migrationen finden Sie unter [Code First-Migrationen](https://msdn.microsoft.com/library/hh770484.aspx).

Wenn Sie ein Webprojekt bereitstellen, kann Visual Studio den Prozess der Bereitstellung einer Datenbank, die von Code First-Migrationen verwaltet wird, automatisieren. Wenn Sie das Veröffentlichungs Profil erstellen, aktivieren Sie ein Kontrollkästchen mit der Bezeichnung Execute Code First-Migrationen (wird beim Anwendungsstart ausgeführt). Diese Einstellung bewirkt, dass der Bereitstellungs Prozess die Web. config-Datei der Anwendung auf dem Zielserver automatisch konfiguriert, sodass Code First die `MigrateDatabaseToLatestVersion` Initialisierer-Klasse verwendet.

Visual Studio führt während des Bereitstellungs Prozesses keine Aktion mit der Datenbank durch. Wenn die bereitgestellte Anwendung zum ersten Mal nach der Bereitstellung auf die Datenbank zugreift, erstellt Code First die Datenbank automatisch oder aktualisiert das Datenbankschema auf die neueste Version. Wenn die Anwendung eine migrationseed-Methode implementiert, wird die-Methode ausgeführt, nachdem die Datenbank erstellt oder das Schema aktualisiert wurde.

In diesem Tutorial verwenden Sie Code First-Migrationen, um die Anwendungsdatenbank bereitzustellen.

### <a name="the-dbdacfx-web-deploy-provider"></a>Der dbdacfx-Web deploy-Anbieter

Für eine SQL Server Datenbank, die nicht von Entity Framework Code First verwaltet wird, können Sie ein Kontrollkästchen mit der Bezeichnung Datenbank aktualisieren auswählen, wenn Sie das Veröffentlichungs Profil konfigurieren. Während der erstmaligen Bereitstellung erstellt der dbdacfx-Anbieter Tabellen und andere Datenbankobjekte in der Zieldatenbank, um Sie mit der Quelldatenbank zu vergleichen. Bei nachfolgenden bereit Stellungen ermittelt der Anbieter die Unterschiede zwischen den Quell-und Ziel Datenbanken und aktualisiert das Schema der Zieldatenbank entsprechend der Quelldatenbank. Standardmäßig führt der Anbieter keine Änderungen aus, die zu Datenverlusten führen, z. b. Wenn eine Tabelle oder eine Spalte gelöscht wird.

Diese Methode automatisiert die Bereitstellung von Daten in Datenbanktabellen nicht, aber Sie können hierfür Skripts erstellen und Visual Studio so konfigurieren, dass Sie während der Bereitstellung ausgeführt werden. Ein weiterer Grund für das Ausführen von Skripts während der Bereitstellung ist das vornehmen von Schema Änderungen, die nicht automatisch ausgeführt werden können, da Sie zu Datenverlusten führen

In diesem Tutorial verwenden Sie den dbdacfx-Anbieter, um die ASP.net-Mitgliedschafts Datenbank bereitzustellen.

## <a name="troubleshooting-during-this-tutorial"></a>Problembehandlung in diesem Tutorial

Wenn während der Bereitstellung ein Fehler auftritt oder wenn die bereitgestellte Site nicht ordnungsgemäß ausgeführt wird, stellen die Fehlermeldungen nicht immer eine offensichtliche Lösung dar. Zur Unterstützung bei einigen häufigen Problem Szenarios ist eine [Referenzseite](troubleshooting.md) zur Problembehandlung verfügbar. Wenn Sie eine Fehlermeldung erhalten oder etwas nicht funktioniert, wenn Sie die Tutorials durchlaufen, achten Sie darauf, dass Sie die Seite Problembehandlung überprüfen.

## <a name="comments-welcome"></a>Begrüßungs Kommentare

Kommentare zu den Tutorials sind willkommen, und wenn das Tutorial aktualisiert wird, werden alle Maßnahmen zur Unterweisung von Korrekturen oder Vorschlägen für Verbesserungen durchgeführt, die in den Lernprogramm Kommentaren bereitgestellt werden.

<a id="prerequisites"></a>

## <a name="prerequisites"></a>Voraussetzungen

Dieses Tutorial wurde für die folgenden Produkte geschrieben:

- Windows 8 oder Windows 7.
- Visual Studio 2012 oder Visual Studio 2012 Express für Web mit [dem neuesten Update](https://go.microsoft.com/fwlink/?LinkId=272486).
- [Azure SDK für Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=254364)

Sie können das Tutorial mithilfe von Visual Studio 2010 SP1 oder Visual Studio 2013 befolgen, aber einige Bildschirmfotos unterscheiden sich, und einige Features unterscheiden sich.

Wenn Sie Visual Studio 2013 verwenden, installieren Sie das [Azure SDK für Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkID=324322).

Wenn Sie Visual Studio 2010 SP1 verwenden, installieren Sie die folgende Software:

- [Azure SDK für Visual Studio 2010](https://go.microsoft.com/fwlink/?LinkID=254269)
- [SQL Server Express localdb](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=SQLLocalDBOnly_11_0)
- [SQL Server Data Tools](https://msdn.microsoft.com/library/hh500335.aspx).

Abhängig davon, wie viele SDK-Abhängigkeiten bereits auf Ihrem Computer vorhanden sind, kann die Installation des Azure SDK lange dauern (von einigen Minuten bis zu einer halben Stunde oder mehr). Sie benötigen das Azure SDK auch dann, wenn Sie beabsichtigen, anstelle von Azure in einem Hosting-Anbieter eines Drittanbieters zu veröffentlichen, da das SDK die neuesten Updates für Webveröffentlichungs Funktionen von Visual Studio enthält.

> [!NOTE]
> Dieses Tutorial wurde mit der Version 1.8.1 des Azure SDK verfasst. Seitdem wurden neuere Versionen mit zusätzlichen Features veröffentlicht. Die Tutorials wurden aktualisiert, um diese Features zu erwähnen und Links zu Ressourcen zu finden, die weitere Informationen über diese Features enthalten.

Die Anweisungen und Screenshots basieren auf Windows 8, aber in den Tutorials werden die Unterschiede für Windows 7 erläutert.

Zum Abschluss des Tutorials ist eine andere Software erforderlich, aber Sie müssen diese noch nicht installiert haben. Das Tutorial führt Sie durch die Schritte zur Installation, wenn Sie es benötigen.

## <a name="download-the-sample-application"></a>Herunterladen der Beispielanwendung

Die Anwendung, die Sie bereitstellen, hat die Bezeichnung "Configuration University" und wurde bereits für Sie erstellt. Dabei handelt es sich um eine vereinfachte Version einer University-Website, die in den [Entity Framework Tutorials auf der ASP.NET-Website](https://asp.net/entity-framework/tutorials)beschrieben wird.

Wenn Sie die erforderlichen Komponenten installiert haben, laden Sie die [Webanwendung](https://go.microsoft.com/fwlink/p/?LinkId=282627)der "Web. Die *ZIP* -Datei enthält mehrere Versionen des Projekts. Wenn Sie die Schritte des Tutorials durcharbeiten möchten, beginnen Sie mit dem Projekt, C# das sich im Ordner befindet. Um zu sehen, wie das Projekt am Ende der Tutorials aussieht, öffnen Sie das Projekt im Ordner contosouniversity-End.

Führen Sie die folgenden Schritte aus, um das Projekt für die Arbeit mit den Lernprogramm Schritten vorzubereiten:

1. Speichern Sie die conjesouniversity-Projektmappendateien aus dem Ordner in einem Ordner mit dem Namen condesouniversity in dem C# Ordner, den Sie für die Arbeit mit Visual Studio-Projekten verwenden.

    Standardmäßig handelt es sich hierbei um den folgenden Ordner für Visual Studio 2012:

    `C:\Users\<username>\Documents\Visual Studio 2012\Projects`

    (Für die Bildschirmfotos in diesem Tutorial befindet sich der Projektordner im Stammverzeichnis auf dem `C`:-Laufwerk.)
2. Starten Sie Visual Studio, und öffnen Sie das Projekt.
3. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf die Lösung, und klicken Sie dann auf **enablenuget Package Restore**.
4. Erstellen Sie die Projektmappe.
5. Wenn Sie Kompilierungsfehler erhalten, stellen Sie die nuget-Pakete manuell wieder her:

    1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf die Projekt Mappe, und klicken Sie dann auf **nuget-Pakete für**Projekt Mappe verwalten.
    2. Oben im Dialogfeld **nuget-Pakete verwalten** sehen Sie, dass in **dieser Projekt Mappe einige nuget-Pakete fehlen. Klicken Sie zum Wiederherstellen.** Klicken Sie auf die Schaltfläche **Restore** .
    3. Erstellen Sie die Projektmappe neu.
6. Drücken Sie STRG + F5, um die Anwendung auszuführen.

    Die Anwendung wird auf der Startseite der Configuration Manager-Website geöffnet.

    ![Startseite dev](introduction/_static/image1.png)

    (Möglicherweise gibt es eine Wartezeit, während Visual Studio die SQL Server Express localdb-Instanz startet, und Sie erhalten ggf. einen Timeout Fehler, wenn dieser Vorgang zu lange dauert. Starten Sie das Projekt in diesem Fall erneut.)

Die Website Seiten können über die Menüleiste aufgerufen werden, und Sie können die folgenden Funktionen ausführen:

- Zeigen Sie die Statistik Statistik an (die Seite "Info").
- Anzeigen, bearbeiten, löschen und Hinzufügen von Schülern.
- Anzeigen und Bearbeiten von Kursen.
- Dozenten anzeigen und bearbeiten.
- Anzeigen und Bearbeiten von Abteilungen.

Im folgenden finden Sie Screenshots von einigen repräsentativen Seiten.

![Seite "Studenten" dev](introduction/_static/image2.png)

![Seite "Studenten hinzufügen" dev](introduction/_static/image3.png)

## <a name="review-application-features-that-affect-deployment"></a>Anwendungs Features überprüfen

Die folgenden Funktionen der Anwendung wirken sich darauf aus, wie Sie Sie bereitstellen oder was Sie tun müssen, um Sie bereitzustellen. Diese werden in den folgenden Tutorials der Reihe ausführlicher erläutert.

- Die Configuration Manager-Datenbank verwendet eine SQL Server Datenbank, um Anwendungsdaten wie z. b. Studenten-und Dozenten Namen zu speichern. Die Datenbank enthält eine Mischung aus Testdaten und Produktionsdaten. Wenn Sie eine Bereitstellung in der Produktionsumgebung durchzuführen, müssen Sie die Testdaten ausschließen.
- Die Anwendung verwendet das ASP.NET-Mitgliedschaftssystem, in dem die Benutzerkontoinformationen in einer SQL Server-Datenbank gespeichert werden. Die Anwendung definiert einen Administrator Benutzer, der Zugriff auf einige eingeschränkte Informationen hat. Sie müssen die Mitgliedschafts Datenbank ohne Testkonten, jedoch mit einem Administrator Konto bereitstellen.
- Die Anwendung verwendet das Dienstprogramm zur Fehler Protokollierung und Berichterstellung von Drittanbietern. Dieses Hilfsprogramm wird in einer Assembly bereitgestellt, die mit der Anwendung bereitgestellt werden muss.
- Das Hilfsprogramm für die Fehler Protokollierung Schreibt Fehlerinformationen in XML-Dateien in einen Datei Ordner. Sie müssen sicherstellen, dass das Konto, unter dem ASP.NET ausgeführt wird, über Schreibberechtigungen für diesen Ordner verfügt und dass Sie diesen Ordner von der Bereitstellung ausschließen müssen. (Andernfalls können Fehlerprotokoll Daten aus der Testumgebung in Produktions-und/oder Produktionsfehler Protokolldateien gelöscht werden.)
- Die Anwendung enthält einige Einstellungen, die in der bereitgestellten *Web. config* -Datei in Abhängigkeit von der Zielumgebung (Test, Staging oder Produktion) und anderen Einstellungen, die je nach Buildkonfiguration geändert werden müssen, geändert werden müssen (Debug oder Release).
- Die Visual Studio-Projekt Mappe enthält ein Klassen Bibliotheksprojekt. Nur die Assembly, die von diesem Projekt generiert wird, sollte bereitgestellt werden, nicht das Projekt selbst.

## <a name="summary"></a>Zusammenfassung

In diesem ersten Tutorial der Reihe haben Sie das Visual Studio-Beispiel Projekt heruntergeladen und die Website Features überprüft, die sich auf die Bereitstellung der Anwendung auswirken. In den folgenden Tutorials bereiten Sie sich auf die Bereitstellung vor, indem Sie einige dieser Aktionen einrichten, die automatisch verarbeitet werden sollen. Andere, die Sie manuell erledigen.

> [!div class="step-by-step"]
> [Weiter](preparing-databases.md)
