---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/debugging-stored-procedures-cs
title: Debugging gespeicherter Prozeduren (C#) | Microsoft-Dokumentation
author: rick-anderson
description: Mit Visual Studio Professional-und Team System-Editionen können Sie Haltepunkte festlegen und gespeicherte Prozeduren in SQL Server schrittweise ausführen, sodass das Debuggen gespeichert wird...
ms.author: riande
ms.date: 08/03/2007
ms.assetid: c655c324-2ffa-4c21-8265-a254d79a693d
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/debugging-stored-procedures-cs
msc.type: authoredcontent
ms.openlocfilehash: 12a8500b107345b9cc9ab457016fdef09ca1bb9d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78428391"
---
# <a name="debugging-stored-procedures-c"></a>Definieren von gespeicherten Prozeduren (C#)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_74_CS.zip) oder [PDF herunterladen](debugging-stored-procedures-cs/_static/datatutorial74cs1.pdf)

> Mit Visual Studio Professional und Team System-Editionen können Sie Haltepunkte festlegen und gespeicherte Prozeduren in SQL Server schrittweise ausführen, sodass das Debuggen gespeicherter Prozeduren so einfach wie das Debugging von Anwendungscode In diesem Tutorial werden das direkte Debuggen und das Debuggen von gespeicherten Prozeduren

## <a name="introduction"></a>Einführung

Visual Studio bietet eine umfassende debuggingdarstellung. Mit wenigen Tastatureingaben oder Mausklicks können Haltepunkte verwendet werden, um die Ausführung eines Programms zu verhindern und dessen Zustand und Ablauf Steuerung zu überprüfen. Neben dem Debuggen von Anwendungscode bietet Visual Studio Unterstützung für das Debuggen gespeicherter Prozeduren in SQL Server Ebenso wie Breakpoints innerhalb des Codes einer ASP.NET-Code Behind-Klasse oder einer Klasse für die Geschäftslogik Schicht festgelegt werden können, können Sie diese auch in gespeicherten Prozeduren platzieren.

In diesem Tutorial werden die Schritte zum Durchlaufen von gespeicherten Prozeduren aus dem Server-Explorer in Visual Studio und das Festlegen von Haltepunkten erläutert, die beim Aufrufen der gespeicherten Prozedur von der laufenden ASP.NET-Anwendung getroffen werden.

> [!NOTE]
> Leider können gespeicherte Prozeduren nur in die Professional-und Team Systems-Versionen von Visual Studio eingecheckt und debuggten werden. Wenn Sie Visual Web Developer oder die Standardversion von Visual Studio verwenden, sind Sie Willkommen beim Durchlaufen der zum Debuggen gespeicherter Prozeduren erforderlichen Schritte. diese Schritte können jedoch nicht auf Ihrem Computer repliziert werden.

## <a name="sql-server-debugging-concepts"></a>SQL Server debugkonzepte

Microsoft SQL Server 2005 wurde entwickelt, um die Integration mit der [Common Language Runtime (CLR)](https://msdn.microsoft.com/netframework/aa497266.aspx)zu ermöglichen, die von allen .NET-Assemblys verwendet wird. Folglich unterstützt SQL Server 2005 verwaltete Datenbankobjekte. Das heißt, Sie können Datenbankobjekte wie gespeicherte Prozeduren und benutzerdefinierte Funktionen (User-Defined Functions, UDFs) als Methoden in einer C# Klasse erstellen. Dadurch können diese gespeicherten Prozeduren und UDFs Funktionen in den .NET Framework und ihren eigenen benutzerdefinierten Klassen nutzen. SQL Server 2005 bietet auch Unterstützung für T-SQL-Datenbankobjekte.

SQL Server 2005 bietet Debugunterstützung sowohl für T-SQL-als auch für verwaltete Datenbankobjekte. Allerdings können diese Objekte nur über die Editionen Visual Studio 2005 Professional und Team Systems deentschlbelt werden. In diesem Tutorial untersuchen wir das Debuggen von T-SQL-Datenbankobjekten. Im nachfolgenden Tutorial wird das Debuggen verwalteter Datenbankobjekte beleuchtet

Die [Übersicht über das T-SQL-und CLR-Debuggen in SQL Server 2005-](https://blogs.msdn.com/sqlclr/archive/2006/06/29/651644.aspx) Blogeintrag aus dem [SQL Server 2005 CLR-Integrationsteam](https://blogs.msdn.com/sqlclr/default.aspx) zeigt die drei Möglichkeiten zum Debuggen von SQL Server 2005-Objekten aus Visual Studio:

- **Direct Database Debugging (DDD)** : von Server-Explorer aus können wir alle T-SQL-Datenbankobjekte (z. b. gespeicherte Prozeduren und UDFs) in Einzelschritten ausführen. In Schritt 1 wird DDD untersucht.
- **Anwendungs Debuggen** : Wir können Breakpoints innerhalb eines Datenbankobjekts festlegen und dann unsere ASP.NET-Anwendung ausführen. Wenn das Datenbankobjekt ausgeführt wird, wird der Breakpoint gedrückt und die Steuerung wird an den Debugger übergeben. Beachten Sie, dass wir beim Anwendungs Debuggen nicht aus dem Anwendungscode in ein Datenbankobjekt springen können. In diesen gespeicherten Prozeduren oder UDFs müssen die Haltepunkte explizit festgelegt werden, wenn der Debugger angehalten werden soll. Das Debuggen von Anwendungen wird ab Schritt 2 untersucht.
- Das **Debuggen von einer SQL Server Projekt** -Visual Studio Professional und Team System-Editionen umfasst einen SQL Server Projekttyp, der häufig zum Erstellen von verwalteten Datenbankobjekten verwendet wird. Im nächsten Tutorial wird die Verwendung von SQL Server Projekten und das Debuggen Ihres Inhalts untersucht.

Visual Studio kann gespeicherte Prozeduren auf lokalen und Remote SQL Server Instanzen Debuggen. Eine lokale SQL Server Instanz ist eine Instanz, die auf demselben Computer wie Visual Studio installiert ist. Wenn sich die SQL Server Datenbank, die Sie verwenden, nicht auf dem Entwicklungs Computer befindet, wird Sie als Remote Instanz betrachtet. Für diese Tutorials verwenden wir lokale SQL Server Instanzen. Das Debuggen gespeicherter Prozeduren auf einer Remote Instanz von SQL Server erfordert mehr Konfigurationsschritte als beim Debuggen gespeicherter Prozeduren auf einer

Wenn Sie eine lokale SQL Server-Instanz verwenden, können Sie mit Schritt 1 beginnen und das Tutorial am Ende durcharbeiten. Wenn Sie jedoch eine Remote SQL Server-Instanz verwenden, müssen Sie zunächst sicherstellen, dass Sie beim Debuggen auf Ihrem Entwicklungs Computer mit einem Windows-Benutzerkonto angemeldet sind, das über eine SQL Server Anmeldung auf der Remote Instanz verfügt. Außerdem müssen sowohl dieser Daten Bank Anmelde Name als auch der Daten Bank Anmelde Name, der zum Herstellen der Verbindung mit der Datenbank aus der laufenden ASP.NET-Anwendung verwendet wird, Mitglieder der `sysadmin` Rolle sein. Weitere Informationen zum Konfigurieren von Visual Studio und SQL Server zum Debuggen einer Remote Instanz finden Sie im Abschnitt Debuggen von T-SQL-Datenbankobjekten auf Remote Instanzen am Ende dieses Tutorials.

Beachten Sie, dass die Debugunterstützung für T-SQL-Datenbankobjekte nicht so umfangreich ist wie die Debugunterstützung für .NET-Anwendungen. Haltepunkt Bedingungen und Filter werden z. b. nicht unterstützt, nur eine Teilmenge der Debuggingfenster ist verfügbar, Sie können "Bearbeiten und Fortfahren" nicht verwenden, das direkt Fenster wird als nutzlos gerendert usw. Weitere Informationen finden Sie [unter Einschränkungen bei Debugger-Befehlen und-Funktionen](https://msdn.microsoft.com/library/ms165035(VS.80).aspx) .

## <a name="step-1-directly-stepping-into-a-stored-procedure"></a>Schritt 1: direktes durchlaufen einer gespeicherten Prozedur

Visual Studio erleichtert das direkte Debuggen eines Datenbankobjekts. Sehen Sie sich an, wie Sie das Feature "Direct Database Debugging" (DDD) verwenden, um die gespeicherte Prozedur `Products_SelectByCategoryID` in der Northwind-Datenbank schrittweise auszuführen. Wie der Name schon sagt, gibt `Products_SelectByCategoryID` Produktinformationen für eine bestimmte Kategorie zurück. Es wurde im Tutorial [Verwenden vorhandener gespeicherter Prozeduren für das typisierte DataSet s TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) erstellt. Wechseln Sie zunächst zum Server-Explorer, und erweitern Sie den Datenbankknoten Northwind. Führen Sie als nächstes einen Drilldown zum Ordner gespeicherte Prozeduren aus, klicken Sie mit der rechten Maustaste auf die gespeicherte Prozedur `Products_SelectByCategoryID`, und wählen Sie im Kontextmenü die Option Einzelschritt in gespeicherte Prozedur aus. Dadurch wird der Debugger gestartet.

Da die gespeicherte Prozedur `Products_SelectByCategoryID` einen `@CategoryID` Eingabeparameter erwartet, werden wir aufgefordert, diesen Wert anzugeben. Geben Sie 1 ein, wodurch Informationen zu den Getränken zurückgegeben werden.

![Verwenden Sie den Wert 1 für den @CategoryID-Parameter.](debugging-stored-procedures-cs/_static/image1.png)

**Abbildung 1**: Verwenden des Werts 1 für den `@CategoryID`-Parameter

Nachdem der Wert für den `@CategoryID`-Parameter angegeben wurde, wird die gespeicherte Prozedur ausgeführt. Anstatt bis zum Abschluss auszuführen, hält der Debugger die Ausführung bei der ersten Anweisung an. Notieren Sie sich den gelben Pfeil im Rand, der den aktuellen Speicherort in der gespeicherten Prozedur angibt. Sie können Parameterwerte über die Überwachungsfenster anzeigen und bearbeiten oder indem Sie in der gespeicherten Prozedur auf den Parameternamen zeigen.

[![der Debugger bei der ersten Anweisung der gespeicherten Prozedur angehalten wurde.](debugging-stored-procedures-cs/_static/image3.png)](debugging-stored-procedures-cs/_static/image2.png)

**Abbildung 2**: der Debugger wurde bei der ersten Anweisung der gespeicherten Prozedur angehalten ([Klicken Sie, um das Bild in voller Größe anzuzeigen](debugging-stored-procedures-cs/_static/image4.png))

Klicken Sie in der Symbolleiste auf die Schaltfläche Prozedur Schritt, oder drücken Sie die Taste F10, um die gespeicherte Prozedur schrittweise durchlaufen. Die gespeicherte Prozedur `Products_SelectByCategoryID` enthält eine einzige `SELECT` Anweisung. durch Drücken von F10 wird also die einzige Anweisung überschritten, und die Ausführung der gespeicherten Prozedur wird beendet. Nachdem die gespeicherte Prozedur abgeschlossen wurde, wird die Ausgabe im Fenster Ausgabe angezeigt, und der Debugger wird beendet.

> [!NOTE]
> T-SQL-Debugging erfolgt auf Anweisungs Ebene. Sie können eine `SELECT` Anweisung nicht in Einzelschritten ausführen.

## <a name="step-2-configuring-the-website-for-application-debugging"></a>Schritt 2: Konfigurieren der Website für das Debuggen von Anwendungen

Das Debuggen einer gespeicherten Prozedur direkt aus der Server-Explorer ist praktisch, aber in vielen Szenarios sind Sie eher daran interessiert, die gespeicherte Prozedur zu debuggen, wenn Sie von der ASP.NET-Anwendung aufgerufen wird. Wir können einer gespeicherten Prozedur Haltepunkte in Visual Studio hinzufügen und dann mit dem Debuggen der ASP.NET-Anwendung beginnen. Wenn eine gespeicherte Prozedur mit Breakpoints von der Anwendung aufgerufen wird, wird die Ausführung am Haltepunkt angehalten, und wir können die Parameterwerte der gespeicherten Prozedur anzeigen und ändern und die zugehörigen Anweisungen schrittweise durchlaufen, wie in Schritt 1.

Bevor wir mit dem Debuggen von gespeicherten Prozeduren beginnen können, die von der Anwendung aufgerufen werden, müssen wir die ASP.NET-Webanwendung anweisen, in den SQL Server Debugger Klicken Sie zunächst mit der rechten Maustaste auf den Namen der Website im Projektmappen-Explorer (`ASPNET_Data_Tutorial_74_CS`). Wählen Sie im Kontextmenü die Option Eigenschaften Seiten aus, wählen Sie auf der linken Seite die Option Start Optionen aus, und aktivieren Sie das Kontrollkästchen SQL Server im Abschnitt Debuggers (siehe Abbildung 3).

[![aktivieren Sie das Kontrollkästchen SQL Server in den Eigenschaften Seiten der Anwendung.](debugging-stored-procedures-cs/_static/image6.png)](debugging-stored-procedures-cs/_static/image5.png)

**Abbildung 3**: Aktivieren Sie das Kontrollkästchen "SQL Server" in den Eigenschaften Seiten der Anwendung ([Klicken Sie, um das Bild in voller Größe anzuzeigen](debugging-stored-procedures-cs/_static/image7.png)).

Außerdem müssen wir die Daten bankverbindungs Zeichenfolge aktualisieren, die von der Anwendung verwendet wird, damit das Verbindungspooling deaktiviert ist. Wenn eine Verbindung mit einer Datenbank geschlossen wird, wird das entsprechende `SqlConnection` Objekt in einem Pool verfügbarer Verbindungen platziert. Beim Herstellen einer Verbindung mit einer Datenbank kann ein verfügbares Verbindungs Objekt aus diesem Pool abgerufen werden, anstatt eine neue Verbindung erstellen und einrichten zu müssen. Dieses Pooling von Verbindungs Objekten ist eine Leistungsverbesserung und ist standardmäßig aktiviert. Beim Debuggen soll das Verbindungspooling jedoch deaktiviert werden, da die debugginginfrastruktur nicht ordnungsgemäß wieder hergestellt wird, wenn eine Verbindung hergestellt wird, die aus dem Pool entnommen wurde.

Um das Verbindungspooling zu deaktivieren, aktualisieren Sie das `NORTHWNDConnectionString` in `Web.config`, sodass es die Einstellung `Pooling=false` enthält.

[!code-xml[Main](debugging-stored-procedures-cs/samples/sample1.xml)]

> [!NOTE]
> Wenn Sie das debugSQL Server gen durch die ASP.NET-Anwendung abgeschlossen haben, stellen Sie sicher, dass Sie das Verbindungspooling wiederherstellen, indem Sie die `Pooling` Einstellung aus der Verbindungs Zeichenfolge entfernen (oder indem Sie Sie auf `Pooling=true`

An diesem Punkt wurde die ASP.NET-Anwendung so konfiguriert, dass Visual Studio SQL Server Datenbankobjekte Debuggen kann, wenn Sie über die Webanwendung aufgerufen wird. Jetzt müssen Sie nur noch einen Haltepunkt zu einer gespeicherten Prozedur hinzufügen und mit dem Debuggen beginnen!

## <a name="step-3-adding-a-breakpoint-and-debugging"></a>Schritt 3: Hinzufügen eines Breakpoints und Debuggens

Öffnen Sie die gespeicherte Prozedur `Products_SelectByCategoryID`, und legen Sie am Anfang der `SELECT` Anweisung einen Haltepunkt fest, indem Sie am entsprechenden Speicherort auf den Rand klicken oder den Cursor am Anfang der `SELECT`-Anweisung platzieren und F9 drücken. Wie in Abbildung 4 dargestellt, wird der Breakpoint als roter Kreis im Rand angezeigt.

[![einen Haltepunkt in der Products_SelectByCategoryID gespeicherten Prozedur fest.](debugging-stored-procedures-cs/_static/image9.png)](debugging-stored-procedures-cs/_static/image8.png)

**Abbildung 4**: Festlegen eines Breakpoints in der gespeicherten Prozedur `Products_SelectByCategoryID` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](debugging-stored-procedures-cs/_static/image10.png))

Damit ein SQL-Datenbankobjekt über eine Client Anwendung debuggt werden kann, ist es zwingend erforderlich, dass die Datenbank so konfiguriert wird, dass das Anwendungs Debugging unterstützt wird. Wenn Sie zum ersten Mal einen Haltepunkt festlegen, sollte diese Einstellung automatisch aktiviert werden, aber es ist ratsam, Sie zu überprüfen. Klicken Sie mit der rechten Maustaste auf den Knoten `NORTHWND.MDF` in der Server-Explorer. Das Kontextmenü sollte ein aktiviertes Menü Element namens Anwendungs Debuggen enthalten.

![Sicherstellen, dass die Option zum Debuggen von Anwendungen](debugging-stored-procedures-cs/_static/image11.png)

**Abbildung 5**: sicherstellen, dass die Option zum Debuggen von Anwendungen aktiviert

Wenn die Haltepunkt Gruppe und die Option zum Debuggen von Anwendungen aktiviert sind, können Sie die gespeicherte Prozedur Debuggen, wenn Sie von der ASP.NET-Anwendung aufgerufen wird. Starten Sie den Debugger, indem Sie im Menü Debuggen die Option Debuggen starten auswählen, drücken Sie F5, oder klicken Sie auf das grüne Wiedergabe Symbol auf der Symbolleiste. Dadurch wird der Debugger gestartet und die Website gestartet.

Die gespeicherte Prozedur `Products_SelectByCategoryID` wurde im Tutorial [Verwenden vorhandener gespeicherter Prozeduren für das typisierte DataSet s TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) erstellt. Die zugehörige Webseite (`~/AdvancedDAL/ExistingSprocs.aspx`) enthält eine GridView, in der die von dieser gespeicherten Prozedur zurückgegebenen Ergebnisse angezeigt werden. Besuchen Sie diese Seite über den Browser. Wenn Sie die Seite erreichen, wird der Breakpoint in der `Products_SelectByCategoryID` gespeicherten Prozedur erreicht und die Steuerung an Visual Studio zurückgegeben. Ebenso wie in Schritt 1 können Sie die Anweisungen der gespeicherten Prozedur durchlaufen und die Parameterwerte anzeigen und ändern.

[![auf der Seite existingsprocs. aspx werden zunächst die Getränke angezeigt.](debugging-stored-procedures-cs/_static/image13.png)](debugging-stored-procedures-cs/_static/image12.png)

**Abbildung 6**: auf der Seite "`ExistingSprocs.aspx`" werden zunächst die Getränke angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](debugging-stored-procedures-cs/_static/image14.png))

[![der Breakpoint der gespeicherten Prozedur wurde erreicht.](debugging-stored-procedures-cs/_static/image16.png)](debugging-stored-procedures-cs/_static/image15.png)

**Abbildung 7**: der Breakpoint der gespeicherten Prozedur wurde erreicht ([Klicken Sie, um das Bild in voller Größe anzuzeigen](debugging-stored-procedures-cs/_static/image17.png))

Wie das Überwachungsfenster in Abbildung 7 zeigt, ist der Wert des `@CategoryID`-Parameters 1. Dies liegt daran, dass auf der `ExistingSprocs.aspx` Seite anfänglich Produkte in der Kategorie Getränke angezeigt werden, die den `CategoryID` Wert 1 aufweist. Wählen Sie eine andere Kategorie aus der Dropdown Liste aus. Dies führt zu einem Postback und führt die gespeicherte Prozedur `Products_SelectByCategoryID` erneut aus. Der Breakpoint wird wiederholt, aber dieses Mal gibt der Wert des `@CategoryID`-Parameters den ausgewählten Dropdown Listenelement-`CategoryID`an.

[![wählen Sie eine andere Kategorie aus der Dropdown Liste aus.](debugging-stored-procedures-cs/_static/image19.png)](debugging-stored-procedures-cs/_static/image18.png)

**Abbildung 8**: Auswählen einer anderen Kategorie aus der Dropdown Liste ([Klicken Sie, um das Bild in voller Größe anzuzeigen](debugging-stored-procedures-cs/_static/image20.png))

[![der @CategoryID Parameter die von der Webseite ausgewählte Kategorie widerspiegelt.](debugging-stored-procedures-cs/_static/image22.png)](debugging-stored-procedures-cs/_static/image21.png)

**Abbildung 9**: der `@CategoryID` Parameter spiegelt die von der Webseite ausgewählte Kategorie wider ([Klicken Sie, um das Bild in voller Größe anzuzeigen](debugging-stored-procedures-cs/_static/image23.png))

> [!NOTE]
> Wenn der Haltepunkt in der `Products_SelectByCategoryID` gespeicherten Prozedur beim Aufrufen der `ExistingSprocs.aspx` Seite nicht angezeigt wird, stellen Sie sicher, dass das Kontrollkästchen SQL Server im Abschnitt Debuggers der Eigenschaften Seite der ASP.NET-Anwendung aktiviert ist, dass das Verbindungspooling deaktiviert wurde und dass die Option zum Debuggen von Datenbankanwendungen aktiviert ist. Wenn weiterhin Probleme auftreten, starten Sie Visual Studio neu, und versuchen Sie es erneut.

## <a name="debugging-t-sql-database-objects-on-remote-instances"></a>Debuggen von T-SQL-Datenbankobjekten auf Remote Instanzen

Das Debuggen von Datenbankobjekten über Visual Studio ist recht unkompliziert, wenn sich die SQL Server-Daten Bank Instanz auf demselben Computer wie Visual Studio befindet. Wenn sich SQL Server und Visual Studio jedoch auf unterschiedlichen Computern befinden, ist eine sorgfältige Konfiguration erforderlich, damit alles ordnungsgemäß funktioniert. Es gibt zwei grundlegende Aufgaben, mit denen wir konfrontiert sind:

- Stellen Sie sicher, dass der Anmelde Name, der für die Verbindung mit der Datenbank über ADO.NET verwendet wird, zur `sysadmin` Rolle gehört
- Stellen Sie sicher, dass das Windows-Benutzerkonto, das von Visual Studio auf dem Entwicklungs Computer verwendet wird, ein gültiges SQL Server Anmelde Konto ist, das zur `sysadmin` Rolle gehört.

Der erste Schritt ist relativ unkompliziert. Identifizieren Sie zunächst das Benutzerkonto, das zum Herstellen einer Verbindung mit der Datenbank aus der ASP.NET-Anwendung verwendet wird, und fügen Sie dann von SQL Server Management Studio dieses Anmelde Konto der `sysadmin`-Rolle hinzu.

Die zweite Aufgabe erfordert, dass das Windows-Benutzerkonto, das Sie zum Debuggen der Anwendung verwenden, ein gültiger Anmelde Name in der Remote Datenbank ist. Allerdings ist die Wahrscheinlichkeit, dass das Windows-Konto, mit dem Sie sich bei Ihrer Arbeitsstation angemeldet haben, keine gültige Anmeldung auf SQL Server. Anstatt ihr bestimmtes Anmelde Konto SQL Server hinzuzufügen, empfiehlt es sich, ein Windows-Benutzerkonto als SQL Server Debugging-Konto festzulegen. Wenn Sie dann die Datenbankobjekte einer Remote SQL Server Instanz debuggen möchten, führen Sie Visual Studio mit den Anmelde Informationen für das Windows-Anmelde Konto aus.

Ein Beispiel sollte Ihnen helfen, Dinge zu verdeutlichen. Stellen Sie sich vor, dass in der Windows-Domäne ein Windows-Konto namens `SQLDebug` vorhanden ist. Dieses Konto muss der Remote SQL Server Instanz als gültiger Anmelde Name und als Mitglied der `sysadmin`-Rolle hinzugefügt werden. Um die Remote SQL Server Instanz von Visual Studio aus zu debuggen, müssen wir Visual Studio als `SQLDebug` Benutzer ausführen. Zu diesem Zweck können Sie sich von der Arbeitsstation abmelden, sich als `SQLDebug`wieder anmelden und dann Visual Studio starten. ein einfacherer Ansatz wäre jedoch, sich mit unseren eigenen Anmelde Informationen bei der Arbeitsstation anzumelden und dann `runas.exe` zu starten, um Visual Studio als `SQLDebug` Benutzer zu starten. `runas.exe` ermöglicht die Ausführung einer bestimmten Anwendung im Rahmen eines anderen Benutzerkontos. Wenn Sie Visual Studio als `SQLDebug`starten möchten, können Sie die folgende Anweisung in der Befehlszeile eingeben:

[!code-console[Main](debugging-stored-procedures-cs/samples/sample2.cmd)]

Eine ausführlichere Erläuterung zu diesem Prozess finden Sie unter " [William R. Vaughn](http://betav.com/BLOG/billva/) s" *-Handbuch zu Visual Studio und SQL Server, Siebte Edition* sowie Gewusst wie [: Festlegen von SQL Server Berechtigungen für das Debuggen](https://msdn.microsoft.com/library/w1bhybwz(VS.80).aspx).

> [!NOTE]
> Wenn auf Ihrem Entwicklungs Computer Windows XP Service Pack 2 ausgeführt wird, müssen Sie die Internet Connection Firewall so konfigurieren, dass Remote Debugging zugelassen wird. [Im Artikel Gewusst wie: Aktivieren von SQL Server 2005-Debuggen](https://msdn.microsoft.com/library/s0fk6z6e(VS.80).aspx) wird beschrieben, dass dies zwei Schritte umfasst: (a) auf dem Visual Studio-Host Computer müssen Sie `Devenv.exe` zur Ausnahmeliste hinzufügen und den TCP 135-Port öffnen. und (b) auf dem Remote Computer (SQL) müssen Sie den TCP 135-Port öffnen und der Ausnahmeliste `sqlservr.exe` hinzufügen. Wenn Ihre Domänen Richtlinie die Netzwerkkommunikation über IPSec erfordert, müssen Sie die Ports UDP 4500 und UDP 500 öffnen.

## <a name="summary"></a>Zusammenfassung

Neben der Unterstützung von Debuggingunterstützung für .NET-Anwendungscode bietet Visual Studio auch eine Vielzahl von Debugoptionen für SQL Server 2005. In diesem Tutorial haben wir zwei der folgenden Optionen untersucht: direktes Debuggen von Datenbanken und Anwendungs Debuggen. Zum direkten Debuggen eines T-SQL-Datenbankobjekts suchen Sie das Objekt über die Server-Explorer klicken Sie dann mit der rechten Maustaste darauf, und wählen Sie Einzelschritt aus. Dadurch wird der Debugger gestartet und bei der ersten Anweisung im Datenbankobjekt angehalten. an diesem Punkt können Sie die Objekt-e-Anweisungen schrittweise durchlaufen und Parameterwerte anzeigen und ändern. In Schritt 1 haben wir diese Vorgehensweise verwendet, um die gespeicherte Prozedur `Products_SelectByCategoryID` schrittweise auszuführen.

Beim Anwendungs Debuggen können Breakpoints direkt innerhalb der Datenbankobjekte festgelegt werden. Wenn ein Datenbankobjekt mit Breakpoints von einer Client Anwendung (z. b. einer ASP.NET-Webanwendung) aufgerufen wird, wird das Programm angehalten, während der Debugger Sie übernimmt. Das Debuggen von Anwendungen ist nützlich, da es deutlicher anzeigt, welche Anwendungs Aktion bewirkt, dass ein bestimmtes Datenbankobjekt aufgerufen wird. Es ist jedoch etwas mehr Konfiguration und Einrichtung als das direkte Debuggen von Datenbanken erforderlich.

Datenbankobjekte können auch über SQL Server Projekte deentschlbelt werden. Im nächsten Tutorial wird die Verwendung von SQL Server Projekten und deren Verwendung zum Erstellen und Debuggen verwalteter Datenbankobjekte erläutert.

Fröhliche Programmierung!

## <a name="about-the-author"></a>Zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Zurück](protecting-connection-strings-and-other-configuration-information-cs.md)
> [Weiter](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs.md)
