---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-the-production-web-application-to-use-the-production-database-vb
title: Konfigurieren der produktionsweb Anwendung für die Verwendung der Produktionsdatenbank (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Wie bereits in den vorherigen Tutorials erläutert, ist es nicht ungewöhnlich, dass sich Konfigurationsinformationen zwischen den Entwicklungs-und Produktionsumgebungen unterscheiden. Dies ist es...
ms.author: riande
ms.date: 04/23/2009
ms.assetid: a64a7aa0-6608-449e-83bf-1ef8cceee504
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-the-production-web-application-to-use-the-production-database-vb
msc.type: authoredcontent
ms.openlocfilehash: 7fe4f545a76992ad687827af447d9a9e95bea73f
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74633676"
---
# <a name="configuring-the-production-web-application-to-use-the-production-database-vb"></a>Konfigurieren der Produktionswebanwendung mithilfe der Produktionsdatenbank (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](https://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_08_VB.zip) oder [PDF herunterladen](https://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial08_DBConfig_vb.pdf)

> Wie bereits in den vorherigen Tutorials erläutert, ist es nicht ungewöhnlich, dass sich Konfigurationsinformationen zwischen den Entwicklungs-und Produktionsumgebungen unterscheiden. Dies gilt insbesondere für datengesteuerte Webanwendungen, da sich die Daten bankverbindungs Zeichenfolgen zwischen den Entwicklungs-und Produktionsumgebungen unterscheiden. In diesem Tutorial wird erläutert, wie Sie die Produktionsumgebung so konfigurieren, dass Sie die entsprechende Verbindungs Zeichenfolge enthält.

## <a name="introduction"></a>Einführung

Datengesteuerte Webanwendungen verwenden in der Regel eine andere Datenbank, wenn Sie sich in der Produktionsumgebung befinden. Bei Anwendungen, die von einem Webhostinganbieter gehostet und lokal entwickelt wurden, befindet sich die Entwicklungs Datenbank in der Regel auf dem Entwickler-Computer, während die Produktionsdatenbank auf einem Datenbankserver im Webhosting des Unternehmens gehostet wird. Beim Bereitstellen einer datengesteuerten Webanwendung muss die Entwicklungs Datenbank auf den Produktionsdaten Bankserver kopiert werden. Im vorherigen Tutorial haben wir uns mit den Möglichkeiten zum Ausführen dieses Schritts beschäftigt.

Die-Webanwendung verwendet die Informationen in einer *Verbindungs Zeichenfolge* , um eine Verbindung mit der-Datenbank herzustellen. Die Verbindungs Zeichenfolge, die in der Regel in `Web.config`gespeichert wird, gibt den Namen des Datenbankservers, den Namen der Datenbank, den Sicherheitskontext und weitere Informationen an. Da die von der Webanwendung verwendete Datenbank davon abhängt, ob die Webanwendung in der Entwicklungs-oder in der Produktionsumgebung ausgeführt wird, müssen sich die Verbindungs Zeichenfolgen zwischen den beiden Umgebungen unterscheiden.

Es ist nicht ungewöhnlich, dass Konfigurationsinformationen zwischen den Entwicklungs-und Produktionsumgebungen abweichen. Die *allgemeinen Konfigurations Unterschiede zwischen Entwicklungs-und Produktions-* Tutorial diskutierten Techniken zum Verwalten separater Konfigurationsinformationen zwischen diesen beiden Umgebungen sowie eine kurze Erläuterung zu Daten bankverbindungs Zeichenfolgen. In diesem Tutorial wird erläutert, wie Sie die Produktionsumgebung so konfigurieren, dass Sie die entsprechende Verbindungs Zeichenfolge enthält.

## <a name="examining-the-connection-string-information"></a>Untersuchen der Verbindungs Zeichen folgen Informationen

Die Verbindungs Zeichenfolge, die von der Webanwendung zur Buch Überprüfung verwendet wird, wird in der Konfigurationsdatei der Anwendung `Web.config`gespeichert. `Web.config` enthält einen speziellen Abschnitt zum Speichern von Verbindungs Zeichenfolgen, die [&lt;connectionStrings&gt;](https://msdn.microsoft.com/library/bf7sd233.aspx)passend benannt werden. In der `Web.config`-Datei für die Book Reviews-Website ist eine Verbindungs Zeichenfolge mit dem Namen `ReviewsConnectionString`in diesem Abschnitt definiert:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-vb/samples/sample1.xml)]

Die Verbindungs Zeichenfolge-Data Source = .\sqlexpress; AttachDbFileName = | DataDirectory | \reviews.mdf; integrierte Sicherheit = true; User Instance = true: besteht aus einer Reihe von Optionen und Werten, wobei die Option/Wert-Paare durch ein Semikolon getrennt sind und jede Option und jeder Wert durch ein Gleichheitszeichen getrennt sind. Die vier in dieser Verbindungs Zeichenfolge verwendeten Optionen lauten wie folgt:

- `Data Source`: gibt den Speicherort des Datenbankservers und den Namen der Datenbankserver Instanz an (sofern vorhanden). Der Wert, `.\SQLEXPRESS`, ist ein Beispiel für einen Datenbankserver und einen Instanznamen. Der Zeitraum gibt an, dass sich der Datenbankserver auf demselben Computer wie die Anwendung befindet. der Instanzname ist `SQLEXPRESS`.
- `AttachDbFilename`: gibt den Speicherort der Datenbankdatei an. Der-Wert enthält den Platzhalter `|DataDirectory|`, der zur Laufzeit in den vollständigen Pfad des Ordners der Anwendung `App_Data` aufgelöst wird.
- `Integrated Security`: ein boolescher Wert, der angibt, ob beim Herstellen einer Verbindung mit der Datenbank (false) oder den aktuellen Windows-Konto Anmelde Informationen (true) ein angegebener Benutzername/Kennwort verwendet werden soll.
- `User Instance`: eine Konfigurationsoption, die für die SQL Server Express Editionen spezifisch ist, die angeben, ob nicht-Administratoren auf dem lokalen Computer angefügt werden dürfen, und eine Verbindung mit einer SQL Server Express Editions-Datenbank herstellen. Weitere Informationen zu dieser Einstellung finden Sie unter [SQL Server Express Benutzer Instanzen](https://msdn.microsoft.com/library/ms254504.aspx) .

Die zulässigen Verbindungs Zeichen folgen Optionen sind abhängig von der Datenbank, mit der Sie eine Verbindung herstellen, und dem verwendeten [ADO.net](http://ADO.NET) -Datenbankanbieter. Beispielsweise unterscheidet sich die Verbindungs Zeichenfolge zum Herstellen einer Verbindung mit einer Microsoft SQL Server Datenbank von der für die Verbindung mit einer Oracle-Datenbank verwendeten. Ebenso wird beim Herstellen einer Verbindung mit einer Microsoft SQL Server Datenbank mit dem SqlClient-Anbieter eine andere Verbindungs Zeichenfolge als bei Verwendung des OLE DB-Anbieters verwendet.

Sie können die Daten bankverbindungs Zeichenfolge per Hand erstellen, indem Sie eine Website wie [connectionStrings.com](http://www.connectionstrings.com/) als Ressource verwenden, um die verfügbaren Optionen zu erhalten. Ein einfacherer Ansatz ist jedoch, die Datenbank der Server-Explorer in Visual Studio hinzuzufügen und dann die Verbindungs Zeichenfolge aus der Eigenschaftenfenster abzurufen. Verwenden Sie dieses letztere Verfahren zum Erstellen der Verbindungs Zeichenfolge für den Produktionsdaten Bankserver.

Öffnen Sie Visual Studio, und navigieren Sie zum Fenster Server-Explorer (in Visual Web Developer wird dieses Fenster Datenbank-Explorer genannt). Klicken Sie mit der rechten Maustaste auf die Option Datenverbindungen, und wählen Sie im Kontextmenü die Option Verbindung hinzufügen aus. Dadurch wird der in Abbildung 1 gezeigte Assistent geöffnet. Wählen Sie die entsprechende Datenquelle aus, und klicken Sie auf Weiter.

[![eine neue Datenbank zum Server-Explorer hinzufügen.](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image2.jpg)](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image1.jpg) 

**Abbildung 1**: auswählen, dem Server-Explorer eine neue Datenbank hinzuzufügen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image3.jpg))

Geben Sie als nächstes die verschiedenen Daten bankverbindungs Informationen an (siehe Abbildung 2). Wenn Sie sich bei Ihrem Webhostingunternehmen registriert haben, sollten Sie Informationen zum Herstellen einer Verbindung mit der Datenbank bereitgestellt haben: den Namen des Datenbankservers, den Datenbanknamen, den Benutzernamen und das Kennwort für die Verbindung mit der Datenbank usw. Nachdem Sie diese Informationen eingegeben haben, klicken Sie auf OK, um den Assistenten abzuschließen und die Datenbank dem Server-Explorer hinzuzufügen.

[![geben Sie die Daten bankverbindungs Informationen an.](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image5.jpg)](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image4.jpg) 

**Abbildung 2**: Angeben der Daten bankverbindungs Informationen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image6.jpg))

Die Datenbank der Produktionsumgebung sollte nun in der Server-Explorer aufgeführt werden. Wählen Sie die Datenbank aus der Server-Explorer aus, und wechseln Sie zum Eigenschaftenfenster. Dort finden Sie eine Eigenschaft mit dem Namen Verbindungs Zeichenfolge mit der Verbindungs Zeichenfolge der Datenbank. Angenommen, Sie verwenden eine Microsoft SQL Server-Datenbank in der Produktionsumgebung und den SqlClient-Anbieter, sollte Ihre Verbindungs Zeichenfolge in etwa wie folgt aussehen:

<strong>Datenquelle =<em>Servername</em>; Anfangs Katalog =<em>DatabaseName</em>; Sicherheitsinformationen permanent speichern = true; Benutzer-ID =<em>Benutzername</em>; Password =*Kennwort</strong>*

Dabei sind *Server*Name, *DatabaseName*, *username*und *Password* mit den Werten für den Namen des Datenbankservers, den Datenbanknamen und den Benutzernamen und das Kennwort versehen, die Ihnen von Ihrem Webhost Unternehmen bereitgestellt werden.

## <a name="deploying-the-book-reviews-web-application"></a>Bereitstellen der Webanwendung "Book Reviews"

Im vorherigen Tutorial wurde das Kopieren der Entwicklungs Datenbank in die Produktionsumgebung schrittweise erläutert, die Bereitstellung der datengesteuerten Anwendung wurde jedoch nicht untersucht. An diesem Punkt enthält die Produktionsumgebung die Datenbank, verwendet jedoch die Version der Anwendung "Book Reviews" mit statischen Reviews. Wir müssen die neue, datengesteuerte Anwendung zusammen mit den aktualisierten Konfigurationsinformationen auf dem Produktionsserver bereitstellen.

Nehmen Sie sich einen Moment Zeit, um die datengesteuerte Anwendung aus der Entwicklungsumgebung in der Produktionsumgebung bereitzustellen. Dieser Prozess wurde in den vorherigen Tutorials ausführlich behandelt. Wenn Sie ein Aktualisierungs Programm benötigen, finden Sie weitere Informationen unter Bereitstellen *Ihrer Website mithilfe eines FTP-Clients* oder unter Bereitstellen *Ihrer Website mithilfe von Visual Studio* -Tutorials. Sie müssen sicherstellen, dass die Verbindungs Zeichenfolge der Produktionsdatenbank in der Produktionsumgebung verwendet wird, was bedeutet, dass eine Alternative `Web.config` Datei bereitgestellt werden muss. Insbesondere das geänderte `Web.config` Datei s `<connectionStrings>`-Element muss die Verbindungs Zeichenfolge für die Produktionsdatenbank enthalten und sollte in etwa wie folgt aussehen:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-vb/samples/sample2.xml)]

Beachten Sie, dass die Verbindungs Zeichenfolge im `<connectionStrings>`-Element denselben Namen hat (`ReviewsConnectionString`), aber jetzt die Verbindungs Zeichenfolge der Produktionsdatenbank anstelle der Verbindungs Zeichenfolge der Entwicklungs Datenbank enthält.

Sofern Sie nicht über einen formalisierten Bereitstellungs Workflow verfügen, ändern Sie entweder die `Web.config` Datei manuell so, dass Sie die Verbindungs Zeichenfolge für die Produktionsdatenbank verwendet, bevor Sie die Bereitstellung durchführen (Wiederherstellen der Verbindungs Zeichenfolge für die Verbindungs Zeichenfolge für die Entwicklungs Datenbank), oder verwalten Sie eine separate `Web.config` Datei mit den Konfigurationsinformationen für die Produktionsumgebung, die als

> [!NOTE]
> Wenn Sie versehentlich eine `Web.config` Datei bereitstellen, die die Verbindungs Zeichenfolge der Entwicklungs Datenbank enthält, tritt ein Fehler auf, wenn die Anwendung in der Produktionsumgebung versucht, eine Verbindung mit der Datenbank herzustellen. Dieser Fehler ist ein `SqlException` mit einer Meldung, die meldet, dass der Server nicht gefunden wurde oder auf den nicht zugegriffen werden konnte.

Nachdem der Standort in der Produktionsumgebung bereitgestellt wurde, besuchen Sie den Produktionsstandort über Ihren Browser. Sie sollten die gleiche Benutzer Darstellung wie bei der lokalen Ausführung der datengesteuerten Anwendung sehen und genießen. Wenn Sie die Website in der Produktionsumgebung besuchen, wird die Website natürlich vom Produktionsdaten Bankserver betrieben, während der Besuch der Website in der Entwicklungsumgebung die Datenbank in der Entwicklung verwendet. Abbildung 3 zeigt die Seite "Sendungen *selbst ASP.NET 3,5 in 24 Stunden* Review" der Website in der Produktionsumgebung (Beachten Sie die URL in der Adressleiste des Browsers).

[![die datengesteuerte Anwendung jetzt für die Produktion verfügbar ist!](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image8.jpg)](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image7.jpg) 

**Abbildung 3**: die datengesteuerte Anwendung ist jetzt in der Produktion verfügbar! ([Klicken Sie, um das Bild in voller Größe anzuzeigen](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image9.jpg))

### <a name="storing-connection-strings-in-a-separate-configuration-file"></a>Speichern von Verbindungs Zeichenfolgen in einer separaten Konfigurationsdatei

Eine gängige Methode zum Verwalten separater Konfigurationsinformationen in den Entwicklungs-und Produktionsumgebungen besteht darin, zwei Versionen des `Web.config`zu haben: eine für die Entwicklungsumgebung und eine für die Produktion. Zum Zeitpunkt der Bereitstellung kann die entsprechende `Web.config` Version in die Produktionsumgebung kopiert werden. Im Idealfall würde dieser Prozess im Rahmen des Bereitstellungs Workflows automatisiert werden.

Anstatt zwei separate `Web.config` Dateien beizubehalten, können Sie optional differenziertere Unterschiede bereitstellen. Die Elemente, die die `Web.config` Datei bilden, können in externen Konfigurationsdateien definiert werden, auf die dann in der `Web.config`-Datei verwiesen wird. Kurz gesagt können Sie eine `Web.config` Datei für beide Umgebungen verwenden, die auf eine databaseconnectionstrings. config-Datei verweist, die die Verbindungs Zeichenfolgen enthält, die von der Anwendung verwendet werden, und für jede Umgebung eindeutig wäre. Ich finde, dass die Trennung der unterschiedlichen Konfigurationsinformationen in separate Dateien eine ausführlichere und einfachere `Web.config` Datei bietet und die Konfigurations Unterschiede zwischen den Entwicklungs-und Produktionsumgebungen klarer beschreibt.

Um dieses Verfahren zu verwenden, erstellen Sie zunächst einen neuen Ordner in der Webanwendung mit dem Namen `ConfigSections`. Fügen Sie als nächstes zwei Dateien zu diesem neuen Ordner namens databaseconnectionstrings. dev. config und databaseconnectionstrings. Production. config hinzu. Kopieren Sie als nächstes das `<connectionStrings>`-Element aus `Web.config` in die Dateien databaseconnectionstrings. dev. config und databaseconnectionstrings. Production. config, und ändern Sie dann die Verbindungs Zeichenfolge in der Datei databaseconnectionstrings. Production. config so, dass Sie die Verbindungs Zeichenfolge für die Produktionsdatenbank angibt. Beispielsweise sollte die Datei databaseconnectionstrings. dev. config nur das `<connectionStrings>`-Element mit einer Verbindungs Zeichenfolge enthalten, die auf die Entwicklungs Datenbank verweist:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-vb/samples/sample3.xml)]

Ebenso sollte die Datei databaseconnectionstrings. Production. config nur ein `<connectionStrings>` Element enthalten, aber eine mit der Verbindungs Zeichenfolge der Produktionsdatenbank.

Erstellen Sie eine Kopie der Datei "databaseconnectionstrings. dev. config", und nennen Sie Sie "databaseconnectionstrings. config".

> [!NOTE]
> Sie können der Konfigurationsdatei einen anderen Namen als databaseconnectionstrings. config benennen, wenn Sie wie z. b. `connectionStrings.config` oder `dbInfo.config`. Achten Sie jedoch darauf, dass Sie die Datei mit einer `.config`-Erweiterung benennen, da `.config` Dateien standardmäßig nicht von der ASP.net-Engine verarbeitet werden. Wenn Sie der Datei etwas anderes (z. b. `connectionStrings.txt`) benennen, könnte ein Benutzer seinen Browser auf [www.yoursite.com/ConfigSettings/connectionStrings.txt](http://www.yoursite.com/ConfigSettings/connectionStrings.txt) und den Inhalt der Datei anzeigen.

An diesem Punkt sollte der `ConfigSections` Ordner drei Dateien enthalten (siehe Abbildung 4). Die Dateien databaseconnectionstrings. dev. config und databaseconnectionstrings. Production. config enthalten die Verbindungs Zeichenfolgen für die Entwicklungs-und Produktionsumgebungen. Die Datei databaseconnectionstrings. config enthält die Verbindungs Zeichenfolgen-Informationen, die von der Webanwendung zur Laufzeit verwendet werden. Folglich sollte die Datei "databaseconnectionstrings. config" mit der Datei "databaseconnectionstrings. dev. config" in der Entwicklungsumgebung identisch sein, während die Datei "databaseconnectionstrings. config" bei der Produktion identisch mit databaseconnectionstrings. Production. config.

[![configabschnitte](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image11.jpg)](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image10.jpg) 

**Abbildung 4**: configabschnitts ([Klicken Sie, um das Bild in voller Größe anzuzeigen](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image12.jpg))

Wir müssen nun `Web.config` anweisen, die Datei "databaseconnectionstrings. config" für den Verbindungs Zeichenfolgen-Speicher zu verwenden. Öffnen Sie `Web.config`, und ersetzen Sie das vorhandene `<connectionStrings>`-Element mit folgendem:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-vb/samples/sample4.xml)]

Das `configSource`-Attribut gibt einen physischen Pfad an, der relativ zur `Web.config` Datei ist. Wenn sich die externe `.config` Datei im selben Verzeichnis befindet wie `Web.config`, legen Sie dieses Attribut auf den Dateinamen der `.config` Datei fest. Wenn sich die Datei in einem Unterverzeichnis befindet, wie es bei databaseconnectionstrings. config der Fall ist, geben Sie den Unterordner mithilfe eines umgekehrten Schrägstrichs an, um die Ordner-und Dateinamen zu begrenzen, wie etwa configsections\databaseconnectionstrings.config.

Mit dieser Änderung enthalten die Entwicklungs-und Produktionsumgebungen dieselbe `Web.config` Datei. Der einzige Unterschied ist die Datei databaseconnectionstrings. config. Kopieren Sie die Datei databaseconnectionstrings. Production. config in die Produktion, und benennen Sie Sie in databaseconnectionstrings. config um. Wenn in der Zukunft Änderungen an der Verbindungs Zeichenfolge der Produktionsdatenbank vorgenommen werden, müssen Sie Sie in der Datei databaseconnectionstrings. Production. config vornehmen und diese Datei dann in die Produktionsumgebung hochladen, indem Sie Sie databaseconnectionstrings. config umbenennen.

> [!NOTE]
> Sie können die Informationen für ein beliebiges `Web.config` Element in einer separaten Datei angeben und das `configSource`-Attribut verwenden, um in `Web.config`auf diese Datei zu verweisen.

## <a name="summary"></a>Summary

Datengesteuerte Anwendungen verwenden in der Regel verschiedene Datenbanken in den Entwicklungs-und Produktionsumgebungen. Folglich müssen die in der Konfiguration der Webanwendung gespeicherten Daten bankverbindungs Zeichenfolgen pro Umgebung eindeutig sein. In diesem Tutorial haben wir erläutert, wie Sie die Verbindungs Zeichenfolge für die Produktionsdatenbank ermitteln und wie Sie eindeutige Informationen zur Verbindungs Zeichenfolge in den beiden Umgebungen verwalten können.

Fröhliche Programmierung!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den in diesem Tutorial behandelten Themen finden Sie in den folgenden Ressourcen:

- [Verbindungszeichenfolgen und Konfigurationsdateien](https://msdn.microsoft.com/library/ms254494.aspx)
- [Daten Bank Konfigurations Zeichenfolgen-Informationen @ connectionStrings.com](http://www.connectionstrings.com/)
- [Verschieben von Einstellungen aus der Datei "Web. config"](http://www.asp101.com/tips/index.asp?id=154)
- [Technische Dokumentation für das &lt;connectionStrings&gt;-Element](https://msdn.microsoft.com/library/bf7sd233.aspx)

> [!div class="step-by-step"]
> [Zurück](deploying-a-database-vb.md)
> [Weiter](configuring-a-website-that-uses-application-services-vb.md)
