---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-a-database-cs
title: Bereitstellen einer DatenbankC#() | Microsoft-Dokumentation
author: rick-anderson
description: Zum Bereitstellen einer ASP.NET-Webanwendung müssen die erforderlichen Dateien und Ressourcen aus der Entwicklungsumgebung in die Produktionsumgebung über gestellt werden. Für da...
ms.author: riande
ms.date: 04/23/2009
ms.assetid: ff537a10-9f1f-43fe-9bcb-3dda161ba8f5
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-a-database-cs
msc.type: authoredcontent
ms.openlocfilehash: 83657be794e1ea31f6ad2f2b4adc274724d60cf2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78518319"
---
# <a name="deploying-a-database-c"></a>Bereitstellen einer Datenbank (C#)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](https://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_07_CS.zip) oder [PDF herunterladen](https://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial07_DeployDB_cs.pdf)

> Zum Bereitstellen einer ASP.NET-Webanwendung müssen die erforderlichen Dateien und Ressourcen aus der Entwicklungsumgebung in die Produktionsumgebung über gestellt werden. Für datengesteuerte Webanwendungen schließt dies das Datenbankschema und die Daten ein. Dieses Tutorial ist der erste in einer Reihe, in der die erforderlichen Schritte zum erfolgreichen Bereitstellen der Datenbank aus der Entwicklungsumgebung für die Produktion erläutert werden.

## <a name="introduction"></a>Einführung

Zum Bereitstellen einer ASP.NET-Webanwendung müssen die erforderlichen Dateien und Ressourcen aus der Entwicklungsumgebung in die Produktionsumgebung über gestellt werden. Im Verlauf der letzten sechs Tutorials haben wir uns mit der Bereitstellung einer einfachen Buch Überprüfungen-Webanwendung beschäftigt. Diese Demo Site umfasste eine Reihe von serverseitigen Ressourcen ASP.NET Seiten, Konfigurationsdateien, eine `Web.sitemap` Datei usw. zusammen mit Client seitigen Ressourcen wie Bilder und CSS-Dateien. Aber wie sieht es mit datengesteuerten Webanwendungen aus? Welche zusätzlichen Schritte müssen ausgeführt werden, um eine Webanwendung bereitzustellen, die eine Datenbank verwendet?

In den nächsten Tutorials befassen wir uns mit den Schritten, die zum Bereitstellen einer datengesteuerten Webanwendung erforderlich sind. In diesem Tutorial wird zunächst erläutert, wie Sie das Schema und die Inhalte einer Datenbank aus der Entwicklungsumgebung in die Produktionsumgebung übernehmen, während das nachfolgende Tutorial die erforderlichen Konfigurationsänderungen untersucht. Im folgenden werden die Herausforderungen beim Bereitstellen einer Datenbank erläutert, die die Anwendungsdienste (Mitgliedschaft, Rollen, Profil usw.) verwendet.

## <a name="examining-the-updated-book-reviews-web-application"></a>Die aktualisierte Buch Reviews-Webanwendung wird untersucht.

Um die Bereitstellung einer datengesteuerten Webanwendung zu veranschaulichen, habe ich die Webanwendung "Book Reviews" von einer einfachen, statischen Website auf eine datengesteuerte Website aktualisiert. Wie zuvor gibt es in diesem Tutorial zwei Versionen der Anwendung: eine, die das Webanwendungs Projekt Modell verwendet, und eine, die das Website Projekt Modell verwendet.

Die aktualisierte Webanwendung "Book Reviews" verwendet eine [SQL Server 2008 Express Edition](https://www.microsoft.com/express/sql/default.aspx) -Datenbank, die im Ordner "Site s `App_Data`" (`~/App_Data/Reviews.mdf`) gespeichert ist. Wenn Sie SQL Server 2008 auf Ihrem Computer installiert haben, sollte die Demo ohne Fehler ausgeführt werden. Wenn Sie über eine ältere Version von verfügen SQL Server können Sie entweder den kostenlosen SQL Server 2008 Express Edition installieren, oder Sie können die in diesem Tutorial verfügbaren Daten Bank Skripts verwenden, um die Datenbank selbst zu erstellen.

Die `Reviews.mdf`-Datenbank enthält vier Tabellen:

- `Genres`: enthält einen Datensatz für jedes Genre, z. b. Technologie, Fiktion und Business.
- `Books`: enthält einen Datensatz für jede Prüfung mit Spalten wie `Title`, `GenreId`, `ReviewDate`und `Review`.
- `Authors`: enthält Informationen zu jedem Autor, der zu einem geprüften Buch beigetragen hat.
- `BooksAuthors`: eine m:n-jointabelle, die angibt, welche Autoren welche Bücher verfasst haben.

Abbildung 1 zeigt ein ERS-Diagramm dieser vier Tabellen.

[![die Datenbank der Webanwendung der Buch Überprüfungen besteht aus vier Tabellen.](deploying-a-database-cs/_static/image2.jpg)](deploying-a-database-cs/_static/image1.jpg) 

**Abbildung 1**: die Datenbank "Book Reviews Web Application s" besteht aus vier Tabellen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](deploying-a-database-cs/_static/image3.jpg))

Die vorherige Version der Book Reviews-Website verfügte über eine separate ASP.NET-Seite für jedes Buch. Beispielsweise gab es eine Seite mit dem Namen `~/Tech/TYASP35.aspx`, die die Überprüfung für das *ASP.NET 3,5 in 24 Stunden*enthielt. Diese neue datengesteuerte Version der Website verfügt über die in der Datenbank gespeicherten Reviews und eine einzelne ASP.net page, Review. aspx? ID =*bookid*, die die Überprüfung für das angegebene Buch anzeigt. Ebenso gibt es eine Genre. aspx? ID =*GenreID* -Seite, die die überprüften Bücher im angegebenen Genre auflistet.

In Abbildung 2 und 3 werden die `Genre.aspx`-und `Review.aspx` Seiten in Aktion angezeigt. Notieren Sie sich die URL in der Adressleiste für jede Seite. In Abbildung 2 ist er "Genre. aspx? ID = 85d164ba-1123-4c47-82a0-c8ec75de7e0e". Da 85 d164ba-1123-4c47-82a0-c8ec75de7e0e der `GenreId` Wert für das Technologie Genre ist, wird in der Überschrift der Seite "Technologie Überprüfungen" und in der Aufzählung diese Überprüfungen auf der Website aufgelistet, die unter diesem Genre liegen.

[![der Technologie Genre Seite](deploying-a-database-cs/_static/image5.jpg)](deploying-a-database-cs/_static/image4.jpg) 

**Abbildung 2**: die Technologie Genre Seite ([Klicken Sie, um das Bild in voller Größe anzuzeigen](deploying-a-database-cs/_static/image6.jpg))

[![Sie die Überprüfung selbst durch ASP.NET 3,5 in 24 Stunden](deploying-a-database-cs/_static/image8.jpg)](deploying-a-database-cs/_static/image7.jpg) 

**Abbildung 3**: die Überprüfung für " *Lehre selbst" ASP.NET 3,5 in 24 Stunden* ([Klicken Sie, um das Bild in voller Größe anzuzeigen](deploying-a-database-cs/_static/image9.jpg))

Die Webanwendung Book Reviews enthält auch einen Abschnitt Verwaltung, in dem Administratoren Genres, Reviews und Autoreninformationen hinzufügen, bearbeiten und löschen können. Derzeit können alle Besucher auf den Abschnitt "Verwaltung" zugreifen. In einem zukünftigen Tutorial fügen wir Unterstützung für Benutzerkonten hinzu und erlauben autorisierten Benutzern nur die Verwaltungs Seiten.

Wenn Sie die Anwendung Book Reviews herunterladen, denken Sie daran, dass Sie die Bereitstellung einer datengesteuerten Anwendung veranschaulichen müssen. Sie enthält keine bewährten Methoden, soweit es um Anwendungs Entwürfe geht. Beispielsweise gibt es keine separate Datenzugriffs Schicht (Data Access Layer, DAL); die ASP.NET-Seiten kommunizieren direkt mit der Datenbank über das SqlDataSource-Steuerelement oder ADO.NET-Code in Ihren Code-Behind-Klassen. Eine ausführlichere Betrachtung der Erstellung Daten gesteuerter Anwendungen mit einer mehrstufigen Architektur finden Sie in den Tutorials zu [ *Arbeiten mit Daten* ](../../data-access/index.md).

## <a name="databases-on-development-versus-production"></a>Datenbanken bei Entwicklung und Produktion

Wenn Sie die Entwicklung in einer datengesteuerten Webanwendung starten, müssen Sie eine Daten bankverbindungs Zeichenfolge angeben, die die Anwendungsdetails zum Herstellen einer Verbindung mit der Datenbank bereitstellt. Diese Verbindungs Zeichenfolge gibt unter anderem den Datenbankserver, den Datenbanknamen und die Sicherheitsinformationen an. Die von der Anwendung während der Entwicklung verwendete Datenbank unterscheidet sich meistens von der Datenbank, die in der Produktionsumgebung verwendet wird. Es gibt viele Vorteile bei der Verwendung verschiedener Datenbanken für die Entwicklung und die Produktion. Wenn Sie eine andere Datenbank in der Entwicklung haben, müssen Sie sich nicht darum kümmern, Livedaten versehentlich zu ändern oder zu löschen. Außerdem können Sie in dummytestdaten einfügen oder wichtige Änderungen am Datenmodell vornehmen, ohne sich Gedanken über die Auswirkungen auf die Anwendung in der Produktion machen zu müssen. Der Nachteil einer anderen Datenbank in den Entwicklungs-und Produktionsumgebungen besteht darin, dass bei der Bereitstellung der Anwendung die Datenbank und alle relevanten Änderungen am Schema oder an den Daten der Datenbank ebenfalls bereitgestellt werden müssen.

Vor der ersten Bereitstellung gibt es nur eine Instanz der-Datenbank, und diese Instanz befindet sich in der Entwicklungsumgebung. Wenn Sie die Anwendung zum ersten Mal in der Produktionsumgebung bereitstellen, dürfen wir nicht nur die erforderlichen serverseitigen und Client seitigen Dateien kopieren, sondern auch die Datenbank aus der Entwicklungsumgebung in die Produktionsumgebung kopieren. An dieser Stelle stehen wir sofort mit der Webanwendung "Book Reviews". die Datenbank befindet sich im Ordner "`App_Data`" in der Entwicklungsumgebung, wurde jedoch noch nicht in die Produktionsumgebung übermittelt.

Nachdem die Anwendung bereitgestellt wurde, gibt es zwei Kopien der Datenbank. Bei der Anwendungs Reife können neue Funktionen hinzugefügt werden, was eine Änderung des Datenmodells erforderlich macht (z. b. das Hinzufügen neuer Spalten zu vorhandenen Tabellen, das vornehmen von Änderungen an vorhandenen Spalten, das Hinzufügen neuer Tabellen usw.). Bei der nächsten Bereitstellung der Webanwendung müssen die Änderungen, die seit der letzten Bereitstellung in der Entwicklungsumgebung auf die Datenbank angewendet werden, auf die Produktionsdatenbank angewendet werden. Einige Strategien zum Verwalten dieses Prozesses werden in einem zukünftigen Tutorial erläutert. Dieses Tutorial konzentriert sich auf die Bereitstellung der gesamten Datenbank aus der Entwicklungsumgebung für die Produktion.

## <a name="deploying-the-database-to-the-production-environment"></a>Bereitstellen der Datenbank in der Produktionsumgebung

Im restlichen Teil dieses Tutorials erfahren Sie, wie Sie die Datenbank aus der Entwicklungsumgebung in der Produktionsumgebung bereitstellen. Wenn Sie darauf folgen, müssen Sie sicherstellen, dass Ihr Konto mit Ihrem webhostinstanbieter Microsoft SQL Server Datenbankunterstützung umfasst. Außerdem müssen Sie über einige Informationen verfügen, nämlich den Namen des Datenbankservers, den Datenbanknamen und den Benutzernamen und das Kennwort, die zum Herstellen einer Verbindung mit der Datenbank verwendet werden.

Wie bereits weiter oben in diesem Tutorial erwähnt, ist die Website der Book Reviews-Website eine SQL Server 2008 Express Edition-Datenbank, die im Ordner `App_Data` gespeichert ist. Der Grund dafür ist, dass die Bereitstellung einer solchen Datenbank so einfach wäre wie das Kopieren des `App_Data` Ordners aus der Entwicklungsumgebung in die Produktionsumgebung. Allerdings unterstützen die meisten Webhostinganbieter das Hosting von Datenbanken im `App_Data` Ordner aufgrund von Sicherheitsgründen nicht. Stattdessen stellen Webhosts ein Konto auf einem SQL Server Daten Bank Server in Ihrer Umgebung bereit. Wenn Sie die Datenbank aus Ihrer Entwicklungsumgebung in der Produktionsumgebung bereitstellen, müssen Sie die Datenbank auf dem Datenbankserver des Webhosts registrieren.

Wie können Sie also Ihre Datenbank aus der Entwicklungsumgebung in die Produktionsumgebung bringen? Es gibt mehrere Möglichkeiten, dies zu erreichen, je nachdem, welche Dienste der Webhost anbietet. Bei einigen Hosts, wie z. b. DiscountASP.net, können Sie eine Sicherung der Datenbank oder der tatsächlichen `.mdf` Datei auf Ihrer Website erstellen und dann in der Systemsteuerung die Sicherungsdatei wiederherstellen oder die `.mdf` Datei an den SQL Server Daten Bank Server anfügen. Bei solchen Tools ist die Bereitstellung der Datenbank so einfach wie das Kopieren des `App_Data` Ordners in die Produktionsumgebung und das anschließende anfügen über die Systemsteuerung. Dies ist möglicherweise die einfachste und schnellste Möglichkeit, um Ihre Datenbank zum ersten Mal zu veröffentlichen.

Ein anderer Ansatz ist die Verwendung des Assistenten für die Daten Bank Veröffentlichung. Der Datenbankveröffentlichungs-Assistent ist eine Windows-Desktop Anwendung, die die SQL-Befehle zum Erstellen des s-Datenbankschemas generiert: die Tabellen, gespeicherten Prozeduren, Sichten, benutzerdefinierten Funktionen usw. und optional die Daten in den Tabellen. Sie können dann über SQL Server Management Studio eine Verbindung mit dem Datenbankserver des Webhost Anbieters herstellen und dann dieses Skript ausführen, um die Datenbank in der Produktionsumgebung zu duplizieren. Noch besser: Wenn Ihr webhostinbieter Microsoft s- [Daten Bank Veröffentlichungs Dienste](http://www.codeplex.com/sqlhost/Wiki/View.aspx?title=Database%20Publishing%20Services&amp;referringTitle=Home) unterstützt, können Sie das Skript, das vom Assistenten für die Daten Bank Veröffentlichung generiert wurde, automatisch auf dem Datenbankserver in Ihrem Auftrag ausführen. Der Datenbankveröffentlichungs-Assistent generiert ein Skript, mit dem das Schema und die Daten der Datenbank erstellt werden. es funktioniert unabhängig davon, ob der Webhostanbieter Features wie das Anfügen einer hochgeladenen `.mdf` Datei anbietet.

### <a name="generating-the-sql-commands-to-create-the-database-schema-and-data-using-the-database-publishing-wizard"></a>Erstellen der SQL-Befehle zum Erstellen des Datenbankschemas und der Daten mit dem Datenbankveröffentlichungs-Assistenten

Im folgenden wird erläutert, wie Sie den Datenbankveröffentlichungs-Assistenten zum Bereitstellen der Book Reviews-Datenbank in der Produktion verwenden Wenn Sie Visual Studio 2008 oder höher verwenden, ist der Datenbankveröffentlichungs-Assistent bereits installiert. Wenn Sie Visual Studio 2005 verwenden, müssen Sie zunächst den Assistenten [herunterladen und installieren](https://www.microsoft.com/downloads/details.aspx?familyid=56E5B1C5-BF17-42E0-A410-371A838E570A&amp;displaylang=en) .

Öffnen Sie Visual Studio, und navigieren Sie zur `Reviews.mdf` Datenbank. Wenn Sie Visual Web Developer verwenden, wechseln Sie zum Datenbank-Explorer. Wenn Sie Visual Studio verwenden, verwenden Sie die Server-Explorer. Abbildung 4 zeigt die `Reviews.mdf` Datenbank im Datenbank-Explorer in Visual Web Developer. Wie in Abbildung 4 gezeigt, besteht die `Reviews.mdf` Datenbank aus vier Tabellen, drei gespeicherten Prozeduren und einer benutzerdefinierten Funktion.

[![die Datenbank im Datenbank-Explorer oder Server-Explorer suchen.](deploying-a-database-cs/_static/image11.jpg)](deploying-a-database-cs/_static/image10.jpg) 

**Abbildung 4**: Suchen der Datenbank im Datenbank-Explorer oder Server-Explorer ([Klicken Sie, um das Bild in voller Größe anzuzeigen](deploying-a-database-cs/_static/image12.jpg))

Klicken Sie mit der rechten Maustaste auf den Datenbanknamen, und wählen Sie im Kontextmenü die Option "in Anbieter veröffentlichen" aus. Dadurch wird der Assistent für die Daten Bank Veröffentlichung gestartet (siehe Abbildung 5). Klicken Sie auf Weiter, um den Begrüßungsbildschirm fortzufahren.

[Begrüßungsbildschirm des Assistenten zum Veröffentlichen von Datenbanken ![](deploying-a-database-cs/_static/image14.jpg)](deploying-a-database-cs/_static/image13.jpg) 

**Abbildung 5**: der Begrüßungsbildschirm des Assistenten für den Daten Bank Publishing ([Klicken Sie, um das Bild in voller Größe anzuzeigen](deploying-a-database-cs/_static/image15.jpg))

Auf dem zweiten Bildschirm des Assistenten sind die Datenbanken aufgelistet, auf die der Datenbankveröffentlichungs-Assistent zugreifen kann. Außerdem können Sie auswählen, ob Sie Skripts für alle Objekte in der ausgewählten Datenbank erstellen oder welche Objekte für das Skript Wählen Sie die entsprechende Datenbank aus, und lassen Sie die Option "Skripterstellung für alle Objekte in der ausgewählten Datenbank" aktiviert.

> [!NOTE]
> Wenn Sie die Fehlermeldung "Es sind keine Objekte in der Datenbank Daten *Bankname* der von diesem Assistenten beschreibbaren Typen vorhanden" angezeigt wird, stellen Sie sicher, dass der Pfad zu Ihrer Datenbankdatei nicht übermäßig lang ist, wenn Sie auf dem Bildschirm, der in Abbildung 6 dargestellt ist, auf Weiter klicken Es wurde festgestellt, dass dieser Fehler auftreten kann, wenn der Pfad zur Datenbankdatei zu lang ist.

[Begrüßungsbildschirm des Assistenten zum Veröffentlichen von Datenbanken ![](deploying-a-database-cs/_static/image17.jpg)](deploying-a-database-cs/_static/image16.jpg) 

**Abbildung 6**: der Begrüßungsbildschirm des Assistenten für den Daten Bank Publishing ([Klicken Sie, um das Bild in voller Größe anzuzeigen](deploying-a-database-cs/_static/image18.jpg))

Auf dem nächsten Bildschirm können Sie eine Skriptdatei generieren oder, falls Sie vom Webhost unterstützt wird, die Datenbank direkt auf dem Datenbankserver des Webhost Anbieters veröffentlichen. Wie in Abbildung 7 gezeigt, wird das Skript in die Datei `C:\REVIEWS.MDF.sql`geschrieben.

[![Skripts für die Datenbank in einer Datei, oder veröffentlichen Sie Sie direkt auf dem webhostinstanbieter.](deploying-a-database-cs/_static/image20.jpg)](deploying-a-database-cs/_static/image19.jpg) 

**Abbildung 7**: Erstellen eines Skripts für die Datenbank in einer Datei oder das direkte veröffentlichen auf dem webhostinstanbieter ([Klicken Sie, um das Bild in voller Größe anzuzeigen](deploying-a-database-cs/_static/image21.jpg))

Auf dem nachfolgenden Bildschirm werden Sie aufgefordert, eine Vielzahl von Skript Optionen anzuzeigen. Sie können angeben, ob das Skript DROP-Anweisungen einschließen soll, um diese vorhandenen Objekte zu entfernen. Dies ist standardmäßig auf true festgelegt, was in Ordnung ist, wenn eine Datenbank zum ersten Mal bereitgestellt wird. Sie können auch angeben, ob die Zieldatenbank SQL Server 2000, SQL Server 2005 oder SQL Server 2008 ist. Schließlich können Sie angeben, ob ein Skript für das Schema und die Daten, nur für die Daten oder nur für das Schema ausgeführt werden soll. Das Schema ist die Auflistung von Datenbankobjekten, den Tabellen, gespeicherten Prozeduren, Sichten usw. Die Daten sind die Informationen, die sich in den Tabellen befinden.

Wie Abbildung 8 zeigt, habe ich den Assistenten so konfiguriert, dass er vorhandene Datenbankobjekte löscht, Skripts für eine SQL Server 2008-Datenbank generiert und sowohl das Schema als auch die Daten veröffentlicht.

[![geben Sie die Veröffentlichungs Optionen an.](deploying-a-database-cs/_static/image23.jpg)](deploying-a-database-cs/_static/image22.jpg) 

**Abbildung 8**: Angeben der Veröffentlichungs Optionen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](deploying-a-database-cs/_static/image24.jpg))

In den letzten zwei Bildschirmen werden die Aktionen zusammengefasst, die ausgeführt werden sollen, und dann der Status der Skripterstellung angezeigt. Das Ergebnis der Ausführung des Assistenten besteht darin, dass eine Skriptdatei vorhanden ist, die die SQL-Befehle enthält, die erforderlich sind, um die Datenbank in der Produktionsumgebung zu erstellen und mit denselben Daten wie bei der Entwicklung aufzufüllen.

### <a name="executing-the-sql-commands-on-the-production-environment-database"></a>Ausführen der SQL-Befehle für die Produktionsumgebung

Das Skript, das die SQL-Befehle zum Erstellen der Datenbank und der darin enthaltenen Daten enthält, besteht darin, das Skript in der Produktionsdatenbank auszuführen. Einige Webhostinganbieter bieten in der Systemsteuerung ein Textfeld, in dem Sie SQL-Befehle eingeben können, die in der Datenbank ausgeführt werden. Wenn Sie über eine sehr große Skriptdatei verfügen, funktioniert diese Option möglicherweise nicht (beispielsweise die `REVIEWS.MDF.sql` Skriptdatei mit einer Größe von mehr als 425 KB).

Ein besserer Ansatz besteht darin, eine direkte Verbindung mit dem Produktionsdaten Bankserver mithilfe von SQL Server Management Studio (SSMS) herzustellen. Wenn Sie eine nicht-Express-Edition von SQL Server installiert haben, die auf Ihrem Computer installiert ist, ist SSMS wahrscheinlich bereits installiert. Andernfalls können Sie eine kostenlose Kopie von SQL Server Management Studio Express Edition [herunterladen und installieren](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en) .

Starten Sie SSMS, und stellen Sie mithilfe der vom Webhost Anbieter bereitgestellten Informationen eine Verbindung mit dem Datenbankserver des Webhosts her.

[![Herstellen einer Verbindung mit dem Daten Bank Server des Webhost Anbieters](deploying-a-database-cs/_static/image26.jpg)](deploying-a-database-cs/_static/image25.jpg) 

**Abbildung 9**: Herstellen der Verbindung mit dem Daten Bank Server des Webhost Anbieters ([Klicken Sie, um das Bild in voller Größe anzuzeigen](deploying-a-database-cs/_static/image27.jpg))

Erweitern Sie die Registerkarte Datenbanken, und suchen Sie die Datenbank. Klicken Sie in der oberen linken Ecke der Symbolleiste auf die Schaltfläche neue Abfrage, fügen Sie die SQL-Befehle aus der vom Datenbankveröffentlichungs-Assistenten erstellten Skriptdatei ein, und klicken Sie auf die Schaltfläche ausführen, um diese Befehle auf dem Produktionsdaten Bankserver auszuführen. Wenn die Skriptdatei besonders groß ist, kann es mehrere Minuten dauern, bis die Befehle ausgeführt werden.

[![Herstellen einer Verbindung mit dem Daten Bank Server des Webhost Anbieters](deploying-a-database-cs/_static/image29.jpg)](deploying-a-database-cs/_static/image28.jpg) 

**Abbildung 10**: Herstellen der Verbindung mit dem Daten Bank Server des Webhost Anbieters ([Klicken Sie, um das Bild in voller Größe anzuzeigen](deploying-a-database-cs/_static/image30.jpg))

Das ist schon alles! An diesem Punkt wurde die Entwicklungs Datenbank in die Produktionsumgebung dupliziert. Wenn Sie die Datenbank in SSMS aktualisieren, sollten Sie die neuen Datenbankobjekte sehen. Abbildung 11 zeigt die Tabellen, gespeicherten Prozeduren und benutzerdefinierten Funktionen der Produktionsdatenbank, die diese in der Entwicklungs Datenbank widerspiegeln. Und da wir den Datenbankveröffentlichungs-Assistenten angewiesen haben, die Daten zu veröffentlichen, haben die Tabellen der Produktionsdatenbank die gleichen Daten wie die s-Tabellen der Entwicklungs Datenbank zum Zeitpunkt der Ausführung des Assistenten. Abbildung 12 zeigt die Daten in der `Books` Tabelle der Produktionsdatenbank.

[![die Datenbankobjekte in der Produktionsdatenbank dupliziert wurden.](deploying-a-database-cs/_static/image32.jpg)](deploying-a-database-cs/_static/image31.jpg) 

**Abbildung 11**: die Datenbankobjekte wurden in der Produktionsdatenbank dupliziert ([Klicken Sie, um das Bild in voller Größe anzuzeigen](deploying-a-database-cs/_static/image33.jpg))

[![die Produktionsdatenbank die gleichen Daten wie in der Entwicklungs Datenbank enthält](deploying-a-database-cs/_static/image35.jpg)](deploying-a-database-cs/_static/image34.jpg) 

**Abbildung 12**: die Produktionsdatenbank enthält die gleichen Daten wie in der Entwicklungs Datenbank ([Klicken Sie, um das Bild in voller Größe anzuzeigen](deploying-a-database-cs/_static/image36.jpg))

An diesem Punkt haben wir die Entwicklungs Datenbank nur in der Produktionsumgebung bereitgestellt. Wir haben uns noch nicht mit der Bereitstellung der Webanwendung beschäftigt oder untersucht, welche Konfigurationsänderungen erforderlich sind, damit die Anwendung die Produktionsdatenbank verwendet. Diese Probleme werden im nächsten Tutorial behandelt.

## <a name="summary"></a>Zusammenfassung

Zum Bereitstellen einer datengesteuerten Webanwendung muss die während der Entwicklung verwendete Datenbank in die Produktionsumgebung kopiert werden. Viele Webhostinganbieter bieten Tools, um den Prozess der Bereitstellung einer Datenbank zu vereinfachen. Mit DiscountASP.net können Sie z. b. die Datenbank `.mdf` Datei (oder einer Sicherung) per FTP-Datei versehen und die Datenbank dann über die Systemsteuerung an den Datenbankserver anfügen. Eine weitere Option, die unabhängig von den Features Ihres webhostinbieters funktioniert, ist das Tool Microsoft s-Datenbankveröffentlichungs-Assistent, das ein Skript mit SQL-Befehlen generiert, um das Schema und die Daten der Entwicklungs Datenbank zu erstellen. Nachdem das Skript generiert wurde, können Sie es in der Produktionsdatenbank ausführen.

Nun, da sich die Web Application s-Datenbank in der Produktionsumgebung befindet, können wir die Anwendung bereitstellen. Allerdings gibt die Konfigurationsinformationen der Webanwendung die Verbindungs Zeichenfolge für die Datenbank an, und diese Verbindungs Zeichenfolge verweist auf die Entwicklungs Datenbank. Wir müssen diese Verbindungs Zeichenfolgen-Informationen aktualisieren, wenn Sie den Standort in der Produktionsumgebung bereitstellen. Im nächsten Tutorial werden diese Konfigurations Unterschiede erläutert, und es werden die Schritte erläutert, die zum Veröffentlichen der datengesteuerten Book Reviews-Website in der Produktionsumgebung erforderlich sind.

Fröhliche Programmierung!

#### <a name="further-reading"></a>Weitere nützliche Informationen

Weitere Informationen zu den in diesem Tutorial behandelten Themen finden Sie in den folgenden Ressourcen:

- [Herunterladen des Assistenten für die Microsoft SQL Server-Daten Bank Veröffentlichung 1,1](https://www.microsoft.com/downloads/details.aspx?familyid=56E5B1C5-BF17-42E0-A410-371A838E570A&amp;displaylang=en)
- [Herunterladen der Microsoft SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)

> [!div class="step-by-step"]
> [Zurück](core-differences-between-iis-and-the-asp-net-development-server-cs.md)
> [Weiter](configuring-the-production-web-application-to-use-the-production-database-cs.md)
