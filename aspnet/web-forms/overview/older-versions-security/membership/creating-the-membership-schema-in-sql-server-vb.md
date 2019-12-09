---
uid: web-forms/overview/older-versions-security/membership/creating-the-membership-schema-in-sql-server-vb
title: Erstellen des Mitgliedschafts Schemas in SQL Server (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial werden die Verfahren zum Hinzufügen des erforderlichen Schemas zur Datenbank erläutert, um den sqlmembership shipprovider zu verwenden. Danach können wir...
ms.author: riande
ms.date: 01/18/2008
ms.assetid: 112a674d-716f-41a6-99b8-4074d65a54c0
msc.legacyurl: /web-forms/overview/older-versions-security/membership/creating-the-membership-schema-in-sql-server-vb
msc.type: authoredcontent
ms.openlocfilehash: 96fd72d1f368b1f7947ef0a2293161d97aaf7065
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74580843"
---
# <a name="creating-the-membership-schema-in-sql-server-vb"></a>Erstellen des Mitgliedschaftsschemas unter SQL Server (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_04_VB.zip) oder [PDF herunterladen](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial04_MembershipSetup_vb.pdf)

> In diesem Tutorial werden die Verfahren zum Hinzufügen des erforderlichen Schemas zur Datenbank erläutert, um den sqlmembership shipprovider zu verwenden. Danach untersuchen wir die Schlüsseltabellen im Schema und besprechen deren Zweck und Wichtigkeit. In diesem Tutorial wird erläutert, wie einer ASP.NET-Anwendung mitgeteilt wird, welchen Anbieter das Mitgliedschafts Framework verwenden sollte.

## <a name="introduction"></a>Einführung

Die vorherigen beiden Tutorials untersuchten die Verwendung der Formular Authentifizierung, um Website Besucher zu identifizieren. Das Formular Authentifizierungs Framework erleichtert Entwicklern das Anmelden von Benutzern bei einer Website und die Verwendung von Authentifizierungs Tickets über den Seitenbesuch hinweg. Die `FormsAuthentication`-Klasse enthält Methoden zum Erstellen des Tickets und zum Hinzufügen des Tickets zu den Cookies des Besuchers. Der `FormsAuthenticationModule` untersucht alle eingehenden Anforderungen und erstellt und für diejenigen mit einem gültigen Authentifizierungs Ticket eine `GenericPrincipal` und ein `FormsIdentity`-Objekt der aktuellen Anforderung. Bei der Formular Authentifizierung handelt es sich lediglich um einen Mechanismus zum Gewähren eines Authentifizierungs Tickets an einen Besucher bei der Anmeldung und bei nachfolgenden Anforderungen, um das Ticket zu ermitteln und die Identität des Benutzers zu bestimmen. Damit eine Webanwendung Benutzerkonten unterstützt, müssen wir immer noch einen Benutzerspeicher implementieren und Funktionen zum Überprüfen von Anmelde Informationen, Registrieren neuer Benutzer und der unzähligen anderen Benutzerkonto bezogenen Aufgaben hinzufügen.

Vor ASP.NET 2,0 waren Entwickler bei der Implementierung all dieser Aufgaben im Zusammenhang mit dem Benutzerkonto auf dem Hook. Glücklicherweise hat das ASP.net-Team dieses Manko erkannt und das Mitgliedschafts Framework mit ASP.NET 2,0 eingeführt. Das Mitgliedschafts Framework ist eine Reihe von Klassen in den .NET Framework, die eine programmgesteuerte Schnittstelle zum Ausführen von Aufgaben im Zusammenhang mit dem Benutzerkonto bereitstellen. Dieses Framework basiert auf dem [Anbieter Modell](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), das es Entwicklern ermöglicht, eine angepasste Implementierung in eine standardisierte API zu integrieren.

Wie im Lernprogramm <a id="Tutorial1"> </a>zu den [*Sicherheitsgrundlagen und ASP.NET-Unterstützung*](../introduction/security-basics-and-asp-net-support-vb.md) erläutert, werden die .NET Framework mit zwei integrierten Mitgliedschafts Anbietern ausgeliefert: [`ActiveDirectoryMembershipProvider`](https://msdn.microsoft.com/library/system.web.security.activedirectorymembershipprovider.aspx) und [`SqlMembershipProvider`](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx). Wie der Name schon sagt, verwendet die `SqlMembershipProvider` eine Microsoft SQL Server-Datenbank als Benutzerspeicher. Um diesen Anbieter in einer Anwendung verwenden zu können, müssen wir dem Anbieter mitteilen, welche Datenbank als Speicher verwendet werden soll. Wie Sie sich vorstellen können, erwartet die `SqlMembershipProvider`, dass die Benutzerspeicher-Datenbank bestimmte Datenbanktabellen, Sichten und gespeicherte Prozeduren besitzt. Wir müssen dieses erwartete Schema der ausgewählten Datenbank hinzufügen.

In diesem Tutorial werden zunächst Techniken zum Hinzufügen des erforderlichen Schemas zur-Datenbank erläutert, um die `SqlMembershipProvider`zu verwenden. Danach untersuchen wir die Schlüsseltabellen im Schema und besprechen deren Zweck und Wichtigkeit. In diesem Tutorial wird erläutert, wie einer ASP.NET-Anwendung mitgeteilt wird, welchen Anbieter das Mitgliedschafts Framework verwenden sollte.

Fangen wir an!

## <a name="step-1-deciding-where-to-place-the-user-store"></a>Schritt 1: entscheiden, wo der Benutzerspeicher platziert werden soll

Die Daten einer ASP.NET-Anwendung werden häufig in einer Reihe von Tabellen in einer Datenbank gespeichert. Beim Implementieren des `SqlMembershipProvider` Datenbankschemas müssen wir entscheiden, ob das Mitgliedschafts Schema in derselben Datenbank wie die Anwendungsdaten oder in einer alternativen Datenbank platziert werden soll.

Aus den folgenden Gründen wird empfohlen, das Mitgliedschafts Schema in derselben Datenbank wie die Anwendungsdaten zu suchen:

- **Wartbarkeit** : eine Anwendung, deren Daten in einer Datenbank gekapselt sind, ist leichter zu verstehen, zu warten und bereitzustellen, als eine Anwendung, die über zwei separate Datenbanken verfügt.
- **Relationale Integrität** Durchsuchen der Mitgliedschafts bezogenen Tabellen in derselben Datenbank wie die Anwendungs Tabellen ist es möglich, [Foreign Key-Einschränkungen](http://en.wikipedia.org/wiki/Foreign_key) zwischen den primär Schlüsseln in den Mitgliedschafts bezogenen Tabellen und den zugehörigen Anwendungs Tabellen herzustellen.

Die Entkopplung von Benutzerspeicher-und Anwendungsdaten in separate Datenbanken ist nur sinnvoll, wenn Sie über mehrere Anwendungen verfügen, die jeweils separate Datenbanken verwenden, aber einen gemeinsamen Benutzerspeicher freigeben müssen.

### <a name="creating-a-database"></a>Erstellen einer Datenbank

Die Anwendung, die wir seit dem zweiten Tutorial aufgebaut haben, hat noch keine Datenbank benötigt. Wir benötigen jedoch für den Benutzerspeicher jetzt ein solches. Erstellen Sie einen, und fügen Sie diesem dann das für den `SqlMembershipProvider` Anbieter erforderliche Schema hinzu (siehe Schritt 2).

> [!NOTE]
> In dieser tutorialreihe wird eine [Microsoft SQL Server 2005 Express Edition](https://msdn.microsoft.com/sql/Aa336346.aspx) -Datenbank verwendet, um die Anwendungs Tabellen und das `SqlMembershipProvider` Schema zu speichern. Diese Entscheidung wurde aus zwei Gründen getroffen: Erstens, aufgrund der kostenlosen kostenlosen Express Edition, die die am weitesten lesbar zugängliche Version von SQL Server 2005; Zweitens können SQL Server 2005 Express Edition Datenbanken direkt in den `App_Data` Ordner der Webanwendung eingefügt werden. Dadurch wird die Datenbank und die Webanwendung in einer einzigen ZIP-Datei erstellt und ohne spezielle Installationsanweisungen oder Konfigurationsoptionen erneut bereitgestellt. Wenn Sie lieber eine nicht Express Edition-Version von SQL Server befolgen möchten, können Sie es kostenlos tun. Die Schritte sind praktisch identisch. Das `SqlMembershipProvider` Schema funktioniert mit jeder Version von Microsoft SQL Server 2000 und höher.

Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Ordner `App_Data`, und wählen Sie neues Element hinzufügen aus. (Wenn ein `App_Data` Ordner in Ihrem Projekt nicht angezeigt wird, klicken Sie mit der rechten Maustaste auf das Projekt in Projektmappen-Explorer, wählen Sie Ordner hinzufügen aus, und wählen Sie `App_Data`aus.) Wählen Sie im Dialogfeld Neues Element hinzufügen eine neue SQL-Datenbank mit dem Namen `SecurityTutorials.mdf`hinzufügen aus. In diesem Tutorial fügen wir dieser Datenbank das `SqlMembershipProvider` Schema hinzu. in den nachfolgenden Tutorials werden weitere Tabellen erstellt, um die Anwendungsdaten zu erfassen.

[![dem Ordner "App_Data" eine neue SQL-Datenbank mit dem Namen "securitytutorials. mdf" hinzufügen](creating-the-membership-schema-in-sql-server-vb/_static/image2.png)](creating-the-membership-schema-in-sql-server-vb/_static/image1.png)

**Abbildung 1**: Hinzufügen einer neuen SQL-Datenbank mit dem Namen `SecurityTutorials.mdf` Datenbank zum Ordner "`App_Data`" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-the-membership-schema-in-sql-server-vb/_static/image3.png))

Wenn Sie dem `App_Data` Ordner eine Datenbank hinzufügen, wird dieser automatisch in der Datenbank-Explorer angezeigt. (In der nicht Express Edition-Version von Visual Studio wird die Datenbank-Explorer als Server-Explorer bezeichnet.) Wechseln Sie zum Datenbank-Explorer, und erweitern Sie die soeben hinzugefügte `SecurityTutorials` Datenbank. Wenn die Datenbank-Explorer auf dem Bildschirm nicht angezeigt wird, klicken Sie auf das Menü Ansicht, und wählen Sie Datenbank-Explorer aus, oder drücken Sie STRG + ALT + S. Wie in Abbildung 2 gezeigt, ist die `SecurityTutorials` Datenbank leer. Sie enthält keine Tabellen, keine Sichten und keine gespeicherten Prozeduren.

[![die securitytutorials-Datenbank ist zurzeit leer.](creating-the-membership-schema-in-sql-server-vb/_static/image5.png)](creating-the-membership-schema-in-sql-server-vb/_static/image4.png)

**Abbildung 2**: die `SecurityTutorials` Datenbank ist zurzeit leer ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-the-membership-schema-in-sql-server-vb/_static/image6.png))

## <a name="step-2-adding-thesqlmembershipproviderschema-to-the-database"></a>Schritt 2: Hinzufügen des`SqlMembershipProvider`Schemas zur Datenbank

Für den `SqlMembershipProvider` muss ein bestimmter Satz von Tabellen, Sichten und gespeicherten Prozeduren in der Benutzerspeicher-Datenbank installiert werden. Diese erforderlichen Datenbankobjekte können mithilfe des [`aspnet_regsql.exe` Tools](https://msdn.microsoft.com/library/ms229862.aspx)hinzugefügt werden. Diese Datei befindet sich im Ordner "`%WINDIR%\Microsoft.Net\Framework\v2.0.50727\`".

> [!NOTE]
> Das `aspnet_regsql.exe` Tool verfügt sowohl über Befehlszeilen Funktionalität als auch über eine grafische Benutzeroberfläche. Die grafische Benutzeroberfläche ist benutzerfreundlicher und ist das, was wir in diesem Tutorial untersuchen werden. Die Befehlszeilenschnittstelle ist nützlich, wenn das Hinzufügen des `SqlMembershipProvider` Schemas automatisiert werden muss, z. b. in Buildskripts oder automatisierten Testszenarien.

Das `aspnet_regsql.exe` Tool wird verwendet, um ASP.net- *Anwendungsdienste* einer angegebenen SQL Server Datenbank hinzuzufügen oder zu entfernen. Die ASP.NET-Anwendungsdienste umfassen die Schemas für die `SqlMembershipProvider` und `SqlRoleProvider`zusammen mit den Schemas für die SQL-basierten Anbieter für andere ASP.NET 2,0-Frameworks. Wir müssen dem `aspnet_regsql.exe` Tool zwei Informationen bereitstellen:

- Ob wir Anwendungsdienste hinzufügen oder entfernen möchten, und
- Die Datenbank, aus der das Anwendungs Dienst Schema hinzugefügt oder entfernt werden soll.

Wenn Sie zur Eingabe der zu verwendenden Datenbank aufgefordert werden, werden wir vom `aspnet_regsql.exe` Tool aufgefordert, den Namen des Servers, auf dem sich die Datenbank befindet, die Sicherheits Anmelde Informationen für die Verbindung mit der Datenbank und den Datenbanknamen anzugeben. Wenn Sie die nicht-Express-Edition von SQL Server verwenden, sollten Sie diese Informationen bereits kennen, da es sich dabei um die gleichen Informationen handelt, die Sie über eine Verbindungs Zeichenfolge bereitstellen müssen, wenn Sie die Datenbank über eine ASP.NET-Webseite verwenden. Es ist jedoch etwas komplizierter, den Server-und Datenbanknamen zu ermitteln, wenn Sie eine SQL Server 2005 Express Edition Datenbank im `App_Data` Ordner verwenden.

Im folgenden Abschnitt wird eine einfache Möglichkeit zum Angeben des Server-und Daten Banknamens für eine SQL Server 2005 Express Edition Datenbank im Ordner `App_Data` untersucht. Wenn Sie SQL Server 2005 Express Edition nicht verwenden, können Sie mit dem Abschnitt Installieren des Anwendungsdienste fortfahren.

### <a name="determining-the-server-and-database-name-for-a-sql-server-2005-express-edition-database-in-theapp_datafolder"></a>Bestimmen des Server-und Daten Banknamens für eine SQL Server 2005 Express Edition Datenbank im Ordner "`App_Data`"

Um das `aspnet_regsql.exe` Tool zu verwenden, müssen wir den Namen des Servers und der Datenbank kennen. Der Servername ist `localhost\InstanceName`. Höchstwahrscheinlich ist der *instanceName* `SQLExpress`. Wenn Sie SQL Server 2005 Express Edition jedoch manuell installiert haben (d. h., Sie haben Sie bei der Installation von Visual Studio nicht automatisch installiert), ist es möglich, dass Sie einen anderen Instanznamen ausgewählt haben.

Der Datenbankname ist etwas schwieriger zu bestimmen. Datenbanken im `App_Data` Ordner verfügen in der Regel über einen Datenbanknamen, der eine [Globally Unique Identifier](http://en.wikipedia.org/wiki/Globally_Unique_Identifier) zusammen mit dem Pfad zur Datenbankdatei enthält. Wir müssen diesen Datenbanknamen bestimmen, um das Anwendungs Dienst Schema über `aspnet_regsql.exe`hinzuzufügen.

Die einfachste Möglichkeit, den Datenbanknamen zu ermitteln, besteht darin, ihn über SQL Server Management Studio zu untersuchen. SQL Server Management Studio stellt eine grafische Benutzeroberfläche zum Verwalten von SQL Server 2005-Datenbanken bereit, wird jedoch nicht mit der Express-Edition von SQL Server 2005 ausgeliefert. Die gute Nachricht ist, dass Sie die kostenlose Express-Edition von SQL Server Management Studio [herunterladen können](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en) .

> [!NOTE]
> Wenn Sie auch eine nicht Express Edition-Version von SQL Server 2005 auf Ihrem Desktop installiert haben, ist die Vollversion der Management Studio wahrscheinlich installiert. Sie können die Vollversion verwenden, um den Datenbanknamen zu ermitteln. Befolgen Sie dabei die unten aufgeführten Schritte für die Express Edition.

Schließen Sie zunächst Visual Studio, um sicherzustellen, dass alle Sperren, die von Visual Studio für die Datenbankdatei erzwungen werden, geschlossen werden. Starten Sie als nächstes SQL Server Management Studio, und stellen Sie eine Verbindung mit der `localhost\InstanceName` Datenbank für SQL Server 2005 Express Edition her. Wie bereits erwähnt, ist die Wahrscheinlichkeit, dass der Instanzname `SQLExpress`ist. Wählen Sie für die Option Authentifizierung die Option Windows-Authentifizierung aus.

[![Herstellen einer Verbindung mit der SQL Server 2005 Express Edition Instanz](creating-the-membership-schema-in-sql-server-vb/_static/image8.png)](creating-the-membership-schema-in-sql-server-vb/_static/image7.png)

**Abbildung 3**: Herstellen einer Verbindung mit der SQL Server 2005 Express Edition Instanz ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-the-membership-schema-in-sql-server-vb/_static/image9.png))

Nach dem Herstellen einer Verbindung mit der SQL Server 2005 Express Edition Instanz werden Management Studio Ordner für die Datenbanken, die Sicherheitseinstellungen, die Server Objekte usw. angezeigt. Wenn Sie die Registerkarte Datenbanken erweitern, sehen Sie, dass die `SecurityTutorials.mdf` Datenbank *nicht* in der Daten Bank Instanz registriert ist. Wir müssen zuerst die Datenbank anfügen.

Klicken Sie mit der rechten Maustaste auf den Ordner Datenbanken, und wählen Sie im Kontextmenü anfügen aus. Dadurch wird das Dialogfeld Datenbanken anfügen angezeigt. Klicken Sie hier auf die Schaltfläche hinzufügen, navigieren Sie zur `SecurityTutorials.mdf` Datenbank, und klicken Sie auf OK. Abbildung 4 zeigt das Dialogfeld Datenbanken anfügen, nachdem die `SecurityTutorials.mdf` Datenbank ausgewählt wurde. Abbildung 5 zeigt die Objekt-Explorer Management Studio, nachdem die Datenbank erfolgreich angefügt wurde.

[![Anfügen der Datenbank "securitytutorials. mdf"](creating-the-membership-schema-in-sql-server-vb/_static/image11.png)](creating-the-membership-schema-in-sql-server-vb/_static/image10.png)

**Abbildung 4**: Anfügen der `SecurityTutorials.mdf` Datenbank ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-the-membership-schema-in-sql-server-vb/_static/image12.png))

[![die Datenbank "securitytutorials. mdf" im Ordner "Datenbanken" angezeigt wird.](creating-the-membership-schema-in-sql-server-vb/_static/image14.png)](creating-the-membership-schema-in-sql-server-vb/_static/image13.png)

**Abbildung 5**: die `SecurityTutorials.mdf` Datenbank wird im Ordner "Datenbanken" angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-the-membership-schema-in-sql-server-vb/_static/image15.png))

Wie in Abbildung 5 zu sehen ist, verfügt die `SecurityTutorials.mdf`-Datenbank über einen Recht abstrusen Namen. Ändern Sie den Namen in einen besser denkbarer (und einfacheren) Namen. Klicken Sie mit der rechten Maustaste auf die Datenbank, wählen Sie im Kontextmenü Umbenennen aus, und benennen Sie Sie `SecurityTutorialsDatabase`um. Dadurch wird der Dateiname nicht geändert, sondern nur der Name, den die Datenbank verwendet, um sich selbst SQL Server zu identifizieren.

[![die Datenbank in securitytutorialsdatabase umbenennen](creating-the-membership-schema-in-sql-server-vb/_static/image17.png)](creating-the-membership-schema-in-sql-server-vb/_static/image16.png)

**Abbildung 6**: Umbenennen der Datenbank in `SecurityTutorialsDatabase`([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-the-membership-schema-in-sql-server-vb/_static/image18.png))

An diesem Punkt kennen wir die Server-und Datenbanknamen für die `SecurityTutorials.mdf` Datenbankdatei: `localhost\InstanceName` bzw. `SecurityTutorialsDatabase`. Nun können Sie die Anwendungsdienste über das `aspnet_regsql.exe` Tool installieren.

### <a name="installing-the-application-services"></a>Installieren des Anwendungsdienste

Um das `aspnet_regsql.exe` Tool zu starten, wechseln Sie zum Startmenü, und wählen Sie ausführen aus. Geben Sie `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\aspnet_regsql.exe` in das Textfeld ein, und klicken Sie auf OK. Alternativ können Sie Windows-Explorer verwenden, um einen Drilldown in den entsprechenden Ordner auszuführen und auf die `aspnet_regsql.exe` Datei zu doppelklicken. Bei beiden Ansätzen werden die gleichen Ergebnisse erzielt.

Wenn Sie das `aspnet_regsql.exe` Tool ohne Befehlszeilenargumente ausführen, wird die grafische Benutzeroberfläche ASP.NET SQL Server Setup-Assistenten gestartet. Der Assistent vereinfacht das Hinzufügen oder Entfernen der ASP.NET-Anwendungsdienste für eine angegebene Datenbank. Auf dem ersten Bildschirm des Assistenten, wie in Abbildung 7 dargestellt, wird der Zweck des Tools beschrieben.

[![den SQL Server ASP.NET-Setup-Assistenten zum Hinzufügen des Mitgliedschafts Schemas verwenden](creating-the-membership-schema-in-sql-server-vb/_static/image20.png)](creating-the-membership-schema-in-sql-server-vb/_static/image19.png)

**Abbildung 7**: Verwenden des ASP.NET-SQL Server Setup-Assistenten zum Hinzufügen des Mitgliedschafts Schemas ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-the-membership-schema-in-sql-server-vb/_static/image21.png))

Im zweiten Schritt des Assistenten werden Sie gefragt, ob Sie die Anwendungsdienste hinzufügen oder entfernen möchten. Da wir die für das `SqlMembershipProvider`erforderlichen Tabellen, Sichten und gespeicherten Prozeduren hinzufügen möchten, wählen Sie die Option SQL Server für Anwendungsdienste konfigurieren aus. Wenn Sie dieses Schema später aus der Datenbank entfernen möchten, führen Sie diesen Assistenten erneut aus, und wählen Sie stattdessen die Option Anwendungs Dienst Informationen aus einer vorhandenen Datenbank entfernen aus.

[Wählen Sie ![die Option SQL Server für Anwendungsdienste konfigurieren aus.](creating-the-membership-schema-in-sql-server-vb/_static/image23.png)](creating-the-membership-schema-in-sql-server-vb/_static/image22.png)

**Abbildung 8**: Auswählen der Option "SQL Server für Anwendungsdienste konfigurieren" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-the-membership-schema-in-sql-server-vb/_static/image24.png))

Im dritten Schritt werden die Datenbankinformationen angezeigt: der Servername, die Authentifizierungsinformationen und der Datenbankname. Wenn Sie zusammen mit diesem Tutorial die `SecurityTutorials.mdf` Datenbank zu `App_Data`hinzugefügt, an `localhost\InstanceName`angefügt und in `SecurityTutorialsDatabase`umbenannt haben, verwenden Sie die folgenden Werte:

- Server: `localhost\InstanceName`
- Windows-Authentifizierung
- Datenbank: `SecurityTutorialsDatabase`

[Datenbankinformationen ![eingeben](creating-the-membership-schema-in-sql-server-vb/_static/image26.png)](creating-the-membership-schema-in-sql-server-vb/_static/image25.png)

**Abbildung 9**: eingeben der Datenbankinformationen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-the-membership-schema-in-sql-server-vb/_static/image27.png))

Nachdem Sie die Datenbankinformationen eingegeben haben, klicken Sie auf Weiter. Im letzten Schritt werden die Schritte zusammengefasst, die ausgeführt werden. Klicken Sie auf Weiter, um die Anwendungsdienste zu installieren, und schließen Sie dann den Assistenten ab.

> [!NOTE]
> Wenn Sie Management Studio verwendet haben, um die Datenbank anzufügen und die Datenbankdatei umzubenennen, stellen Sie sicher, dass Sie die Datenbank trennen und Management Studio schließen, bevor Sie Visual Studio erneut öffnen. Wenn Sie die `SecurityTutorialsDatabase` Datenbank trennen möchten, klicken Sie mit der rechten Maustaste auf den Datenbanknamen, und wählen Sie im Menü Aufgaben die Option trennen aus.

Wenn Sie den Assistenten abgeschlossen haben, kehren Sie zu Visual Studio zurück, und navigieren Sie zum Datenbank-Explorer. Erweitern Sie den Ordner Tabellen. Es sollte eine Reihe von Tabellen angezeigt werden, deren Namen mit dem Präfix `aspnet_`beginnen. Ebenso finden Sie eine Vielzahl von Sichten und gespeicherten Prozeduren unter den Ordnern Sichten und gespeicherte Prozeduren. Diese Datenbankobjekte bilden das Anwendungs Dienst Schema. Die Mitgliedschafts-und Rollen spezifischen Datenbankobjekte werden in Schritt 3 untersucht.

[![einer Datenbank wurden verschiedene Tabellen, Sichten und gespeicherte Prozeduren hinzugefügt.](creating-the-membership-schema-in-sql-server-vb/_static/image29.png)](creating-the-membership-schema-in-sql-server-vb/_static/image28.png)

**Abbildung 10**: eine Vielzahl von Tabellen, Sichten und gespeicherten Prozeduren wurden der Datenbank hinzugefügt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-the-membership-schema-in-sql-server-vb/_static/image30.png))

> [!NOTE]
> Die grafische Benutzeroberfläche des `aspnet_regsql.exe` Tools installiert das gesamte Anwendungs Dienst Schema. Wenn Sie `aspnet_regsql.exe` von der Befehlszeile aus ausführen, können Sie jedoch angeben, welche Komponenten für bestimmte Anwendungsdienste installiert (oder entfernt) werden sollen. Wenn Sie nur die Tabellen, Sichten und gespeicherten Prozeduren hinzufügen möchten, die für die `SqlMembershipProvider`-und `SqlRoleProvider`-Anbieter erforderlich sind, führen Sie `aspnet_regsql.exe` über die Befehlszeile aus. Alternativ können Sie die entsprechende Teilmenge von T-SQL-Skripts, die von `aspnet_regsql.exe`verwendet werden, manuell ausführen. Diese Skripts befinden sich im `WINDIR%\Microsoft.Net\Framework\v2.0.50727\` Ordner mit Namen wie `InstallCommon.sql`, `InstallMembership.sql`, `InstallRoles.sql`, `InstallProfile.sql`, `InstallSqlState.sql`usw.

An diesem Punkt haben wir die Datenbankobjekte erstellt, die vom `SqlMembershipProvider`benötigt werden. Wir müssen jedoch weiterhin das Mitgliedschafts Framework anweisen, dass es die `SqlMembershipProvider` verwenden soll (im Gegensatz zum `ActiveDirectoryMembershipProvider`) und dass die `SqlMembershipProvider` die `SecurityTutorials` Datenbank verwenden sollten. Wir sehen uns an, wie Sie angeben, welcher Anbieter verwendet werden soll, und wie die Einstellungen des ausgewählten Anbieters in Schritt 4 angepasst werden. Zuerst sehen wir uns die soeben erstellten Datenbankobjekte genauer an.

## <a name="step-3-a-look-at-the-schemas-core-tables"></a>Schritt 3: Untersuchen der Kern Tabellen des Schemas

Beim Arbeiten mit den Mitgliedschafts-und Rollen Frameworks in einer ASP.NET-Anwendung werden die Implementierungsdetails vom Anbieter gekapselt. In zukünftigen Tutorials werden diese Frameworks über die `Membership`-und `Roles` Klassen der .NET Framework miteinander in eine Schnittstelle integriert. Bei Verwendung dieser APIs auf hoher Ebene müssen wir uns nicht mit den Details auf niedriger Ebene befassen, wie z. b. welche Abfragen ausgeführt werden oder welche Tabellen durch die `SqlMembershipProvider` und `SqlRoleProvider`geändert werden.

Wir könnten die Mitgliedschafts-und Rollen Frameworks zuverlässig verwenden, ohne das in Schritt 2 erstellte Datenbankschema zu untersucht. Beim Erstellen von Tabellen zum Speichern von Anwendungsdaten müssen wir jedoch möglicherweise Entitäten erstellen, die sich auf Benutzer oder Rollen beziehen. Beim Einrichten von Foreign Key-Einschränkungen zwischen den Anwendungsdaten Tabellen und den Tabellen, die in Schritt 2 erstellt wurden, ist es hilfreich, sich mit den `SqlMembershipProvider`-und `SqlRoleProvider` Schemas vertraut zu machen. In bestimmten seltenen Fällen müssen wir möglicherweise eine Schnittstelle mit den Benutzer-und Rollen speichern direkt auf Datenbankebene (anstelle der `Membership`-oder `Roles`-Klassen) verwenden.

### <a name="partitioning-the-user-store-into-applications"></a>Partitionierung des Benutzer Stores in Anwendungen

Die Mitgliedschafts-und Rollen-Frameworks sind so konzipiert, dass ein einzelner Benutzer und ein Rollen Speicher von vielen verschiedenen Anwendungen gemeinsam genutzt werden können. Eine ASP.NET-Anwendung, die die Mitgliedschafts-oder Rollen Framework verwendet, muss angeben, welche Anwendungs Partition verwendet werden soll. Kurz gesagt können mehrere Webanwendungen dieselben Benutzer-und Rollen Speicher verwenden. Abbildung 11 stellt Benutzer-und Rollen Speicher dar, die in drei Anwendungen partitioniert sind: hrsite, customersite und salessite. Diese drei Webanwendungen verfügen jeweils über eigene eindeutige Benutzer und Rollen, aber ihre Benutzerkonten-und Rollen Informationen werden in den gleichen Datenbanktabellen gespeichert.

[![Benutzerkonten können über mehrere Anwendungen hinweg partitioniert werden.](creating-the-membership-schema-in-sql-server-vb/_static/image32.png)](creating-the-membership-schema-in-sql-server-vb/_static/image31.png)

**Abbildung 11**: Benutzerkonten können über mehrere Anwendungen hinweg partitioniert werden ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-the-membership-schema-in-sql-server-vb/_static/image33.png))

In der `aspnet_Applications` Tabelle werden diese Partitionen definiert. Jede Anwendung, die die Datenbank zum Speichern von Benutzerkontoinformationen verwendet, wird durch eine Zeile in dieser Tabelle dargestellt. Die `aspnet_Applications` Tabelle enthält vier Spalten: `ApplicationId`, `ApplicationName`, `LoweredApplicationName`und `Description`.`ApplicationId` ist vom Typ [`uniqueidentifier`](https://msdn.microsoft.com/library/ms187942.aspx) und ist der Primärschlüssel der Tabelle. `ApplicationName` bietet einen eindeutigen benutzerfreundlichen Namen für jede Anwendung.

Die anderen Mitgliedschafts-und Rollen bezogenen Tabellen werden wieder mit dem `ApplicationId`-Feld in `aspnet_Applications`verknüpft. Beispielsweise verfügt die `aspnet_Users` Tabelle, die einen Datensatz für jedes Benutzerkonto enthält, über ein `ApplicationId` Fremdschlüssel Feld. Ditto für die `aspnet_Roles` Tabelle. Das `ApplicationId` Feld in diesen Tabellen gibt die Anwendungs Partition an, zu der das Benutzerkonto oder die Rolle gehört.

### <a name="storing-user-account-information"></a>Speichern von Benutzerkontoinformationen

Die Benutzerkontoinformationen sind in zwei Tabellen untergebracht: `aspnet_Users` und `aspnet_Membership`. Die `aspnet_Users` Tabelle enthält Felder, die die wesentlichen Benutzerkontoinformationen enthalten. Die drei relevantesten Spalten lauten:

- `UserId`
- `UserName`
- `ApplicationId`

`UserId` ist der Primärschlüssel (und vom Typ `uniqueidentifier`). `UserName` ist vom Typ `nvarchar(256)` und bildet zusammen mit dem Kennwort die Anmelde Informationen des Benutzers. (Das Kennwort eines Benutzers wird in der `aspnet_Membership` Tabelle gespeichert.) `ApplicationId` verknüpft das Benutzerkonto mit einer bestimmten Anwendung in `aspnet_Applications`. In den Spalten `UserName` und `ApplicationId` ist eine zusammengesetzte [`UNIQUE` Einschränkung](https://msdn.microsoft.com/library/ms191166.aspx) vorhanden. Dadurch wird sichergestellt, dass in einer bestimmten Anwendung jeder Benutzername eindeutig ist, aber die gleiche `UserName` in verschiedenen Anwendungen verwendet werden kann.

Die `aspnet_Membership` Tabelle enthält zusätzliche Benutzerkontoinformationen, wie z. b. das Kennwort des Benutzers, die e-Mail-Adresse, Datum und Uhrzeit der letzten Anmeldung usw. Zwischen den Datensätzen in den Tabellen "`aspnet_Users`" und "`aspnet_Membership`" besteht eine eins-zu-eins-Entsprechung. Diese Beziehung wird durch das `UserId`-Feld in `aspnet_Membership`gewährleistet, das als Primärschlüssel der Tabelle dient. Wie die `aspnet_Users` Tabelle enthält `aspnet_Membership` ein `ApplicationId` Feld, das diese Informationen an eine bestimmte Anwendungs Partition bindet.

### <a name="securing-passwords"></a>Sichern von Kenn Wörtern

Die Kenn Wort Informationen werden in der `aspnet_Membership` Tabelle gespeichert. Der `SqlMembershipProvider` ermöglicht das Speichern von Kenn Wörtern in der Datenbank mithilfe einer der folgenden drei Verfahren:

- **Clear** : das Kennwort wird in der Datenbank als Klartext gespeichert. Ich empfehle dringend, diese Option zu verwenden. Wenn die Datenbank kompromittiert ist, wird Sie von einem Hacker gefunden, der eine Hintertür oder einen verärgerten-Mitarbeiter hat, der über Zugriff auf die Datenbank verfügt. die Anmelde Informationen für jeden einzelnen Benutzer sind für das nehmen vorhanden.
- Hash **-Kenn** Wörter werden mithilfe eines unidirektionalen Hash Algorithmus und eines zufällig generierten Salt-Werts Hashwert erstellt. Dieser Hashwert (zusammen mit dem Salt-Wert) wird in der-Datenbank gespeichert.
- **Verschlüsselt** : eine verschlüsselte Version des Kennworts wird in der Datenbank gespeichert.

Die verwendete Kenn Wort Speichermethode hängt von den in `Web.config`angegebenen `SqlMembershipProvider` Einstellungen ab. Wir sehen uns an, wie die `SqlMembershipProvider` Einstellungen in Schritt 4 angepasst werden. Das Standardverhalten ist das Speichern des Hashwerts des Kennworts.

Die für das Speichern des Kennworts Verantwortlichen Spalten sind `Password`, `PasswordFormat`und `PasswordSalt`. `PasswordFormat` ist ein Feld vom Typ `int`, dessen Wert das Verfahren angibt, das zum Speichern des Kennworts verwendet wird: 0 für Clear; 1 für Hashwert; 2 für verschlüsselt. `PasswordSalt` wird unabhängig von der verwendeten Kenn Wort Speichermethode eine zufällig generierte Zeichenfolge zugewiesen. der Wert von `PasswordSalt` wird nur beim Berechnen des Hashwerts des Kennworts verwendet. Zum Schluss enthält die Spalte "`Password`" die eigentlichen Kenn Wort Daten, das nur-Text-Kennwort, der Hashwert des Kennworts oder das verschlüsselte Kennwort.

Tabelle 1 zeigt, wie diese drei Spalten für die verschiedenen Speichertechniken aussehen können, wenn das Kennwort MySecret gespeichert wird. .

| **Speichertechnik&lt;\_o3a\_p/&gt;** | **Kennwort&lt;\_o3a\_p/&gt;** | **PasswordFormat&lt;\_o3a\_p/&gt;** | **PasswordSalt&lt;\_o3a\_p/&gt;** |
| --- | --- | --- | --- |
| Löschen | MySecret! | 0 | tTnkPlesqissc2y2SMEygA = = |
| Hash | 2oxm6szhwbthf gjgkgqsc2ec9zm = | 1 | wFgjUfhdUFOCKQiI61vtiQ = = |
| Verschlüsselt | 62rzgdvhxykkqsmchz0yly7hs6onhpaocyarxv8g0f4cw56oxuu3e7inza9j9bkp | 2 | Lsrzhgs/AA/oqaxglhjnbw = = |

**Tabelle 1**: Beispiel Werte für die Kenn Wort bezogenen Felder, wenn das Kennwort MySecret gespeichert wird.

> [!NOTE]
> Der von der `SqlMembershipProvider` verwendete Verschlüsselungs-oder Hash Algorithmus wird durch die Einstellungen im `<machineKey>`-Element bestimmt. Dieses Konfigurationselement wurde in Schritt 3 des Tutorials <a id="Tutorial3"> </a> [*Konfiguration der Formular Authentifizierung und erweiterte Themen*](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md) erläutert.

### <a name="storing-roles-and-role-associations"></a>Speichern von Rollen und Rollen Zuordnungen

Das Rollen Framework ermöglicht es Entwicklern, eine Gruppe von Rollen zu definieren und anzugeben, welche Benutzer zu welchen Rollen gehören. Diese Informationen werden in der-Datenbank über zwei Tabellen aufgezeichnet: `aspnet_Roles` und `aspnet_UsersInRoles`. Jeder Datensatz in der `aspnet_Roles` Tabelle stellt eine Rolle für eine bestimmte Anwendung dar. Ähnlich wie die `aspnet_Users` Tabelle enthält die `aspnet_Roles` Tabelle drei Spalten, die für unsere Diskussion relevant sind:

- `RoleId`
- `RoleName`
- `ApplicationId`

`RoleId` ist der Primärschlüssel (und vom Typ `uniqueidentifier`). `RoleName` ist vom Typ `nvarchar(256)`. Und `ApplicationId` das Benutzerkonto mit einer bestimmten Anwendung in `aspnet_Applications`verknüpft. Es gibt eine zusammengesetzte `UNIQUE` Einschränkung für die `RoleName`-und `ApplicationId` Spalten, die sicherstellen, dass in einer bestimmten Anwendung jeder Rollenname eindeutig ist.

Die `aspnet_UsersInRoles` Tabelle dient als Zuordnung zwischen Benutzern und Rollen. Es gibt nur zwei Spalten `UserId` und `RoleId` und zusammen bilden Sie einen zusammengesetzten Primärschlüssel.

## <a name="step-4-specifying-the-provider-and-customizing-its-settings"></a>Schritt 4: Angeben des Anbieters und Anpassen der Einstellungen

Alle Frameworks, die das Anbieter Modell unterstützen (z. b. die Mitgliedschafts-und Rollen-Frameworks), fehlen Implementierungsdetails selbst und delegieren diese Aufgabe stattdessen an eine Anbieter Klasse. Im Fall des Mitgliedschafts-Frameworks definiert die `Membership`-Klasse die API zum Verwalten von Benutzerkonten, aber Sie interagiert nicht direkt mit einem Benutzerspeicher. Stattdessen übergeben die Methoden der `Membership` Klasse die Anforderung an den konfigurierten Anbieter. Wir verwenden den `SqlMembershipProvider`. Wie weiß das Mitgliedschafts Framework beim Aufrufen einer der Methoden in der `Membership`-Klasse, dass der Aufruf an den `SqlMembershipProvider`delegiert werden soll?

Die `Membership`-Klasse verfügt über eine [`Providers`-Eigenschaft](https://msdn.microsoft.com/library/system.web.security.membership.providers.aspx) , die einen Verweis auf alle registrierten Anbieter Klassen enthält, die für die Verwendung durch das Mitgliedschafts Framework verfügbar sind. Jeder registrierte Anbieter verfügt über einen zugeordneten Namen und Typ. Der Name bietet eine benutzerfreundliche Möglichkeit, auf einen bestimmten Anbieter in der `Providers` Auflistung zu verweisen, während der Typ die Anbieter Klasse identifiziert. Außerdem kann jeder registrierte Anbieter Konfigurationseinstellungen enthalten. Zu den Konfigurationseinstellungen für das Mitgliedschafts Framework zählen unter anderem `PasswordFormat` und `requiresUniqueEmail`. Eine umfassende Liste der vom `SqlMembershipProvider`verwendeten Konfigurationseinstellungen finden Sie in Tabelle 2.

Der Inhalt der `Providers` Eigenschaft wird durch die Konfigurationseinstellungen der Webanwendung angegeben. Standardmäßig verfügen alle Webanwendungen über einen Anbieter mit dem Namen `AspNetSqlMembershipProvider` vom Typ `SqlMembershipProvider`. Dieser Standard Mitgliedschafts Anbieter wird in `machine.config` (unter `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG`) registriert:

[!code-xml[Main](creating-the-membership-schema-in-sql-server-vb/samples/sample1.xml)]

Wie das obige Markup zeigt, definiert das [`<membership>`-Element](https://msdn.microsoft.com/library/1b9hw62f.aspx) die Konfigurationseinstellungen für das Mitgliedschafts Framework, während das`<providers>` untergeordnete [Element](https://msdn.microsoft.com/library/6d4936ht.aspx) die registrierten Anbieter angibt. Anbieter können mithilfe der [`<add>`](https://msdn.microsoft.com/library/whae3t94.aspx) oder [`<remove>`](https://msdn.microsoft.com/library/aykw9a6d.aspx) Elementen hinzugefügt oder entfernt werden. Verwenden Sie das [`<clear>`](https://msdn.microsoft.com/library/t062y6yc.aspx) -Element, um alle zurzeit registrierten Anbieter zu entfernen. Wie das obige Markup zeigt, fügt `machine.config` einen Anbieter mit dem Namen `AspNetSqlMembershipProvider` vom Typ `SqlMembershipProvider`hinzu.

Zusätzlich zu den `name`-und `type` Attributen enthält das `<add>`-Element Attribute, die die Werte für verschiedene Konfigurationseinstellungen definieren. In Tabelle 2 werden die verfügbaren `SqlMembershipProvider`spezifischen Konfigurationseinstellungen zusammen mit einer Beschreibung aufgeführt.

> [!NOTE]
> Alle in Tabelle 2 notierten Standardwerte verweisen auf die Standardwerte, die in der `SqlMembershipProvider`-Klasse definiert sind. Beachten Sie, dass nicht alle Konfigurationseinstellungen in `AspNetSqlMembershipProvider` den Standardwerten der `SqlMembershipProvider`-Klasse entsprechen. Wenn z. b. in einem Mitgliedschafts Anbieter nicht angegeben wird, wird für die `requiresUniqueEmail` Einstellung standardmäßig true festgelegt. Der `AspNetSqlMembershipProvider` überschreibt diesen Standardwert jedoch durch explizites angeben des Werts `false`.

| **Festlegen von&lt;\_o3a\_p/&gt;** | **Beschreibung&lt;\_o3a\_p/&gt;** |
| --- | --- |
| `ApplicationName` | Beachten Sie, dass das Mitgliedschafts Framework ermöglicht, dass ein einzelner Benutzerspeicher über mehrere Anwendungen hinweg partitioniert werden kann. Diese Einstellung gibt den Namen der Anwendungs Partition an, die vom Mitgliedschafts Anbieter verwendet wird. Wenn dieser Wert nicht explizit angegeben wird, wird er zur Laufzeit auf den Wert des virtuellen Stammpfad der Anwendung festgelegt. |
| `commandTimeout` | Gibt den Timeout Wert für den SQL-Befehl (in Sekunden) an. Der Standardwert ist 30. |
| `connectionStringName` | Der Name der Verbindungs Zeichenfolge im `<connectionStrings>`-Element, das zum Herstellen einer Verbindung mit der Benutzerspeicher-Datenbank verwendet werden soll. Dieser Wert ist erforderlich. |
| `description` | Stellt eine benutzerfreundliche Beschreibung des registrierten Anbieters bereit. |
| `enablePasswordRetrieval` | Gibt an, ob Benutzer ihr vergessenes Kennwort abrufen dürfen. Der Standardwert ist `false`sein. |
| `enablePasswordReset` | Gibt an, ob Benutzer Ihr Kennwort zurücksetzen können. Wird standardmäßig auf `true` festgelegt. |
| `maxInvalidPasswordAttempts` | Die maximale Anzahl von erfolglosen Anmelde versuchen, die für einen bestimmten Benutzer während des angegebenen `passwordAttemptWindow` auftreten können, bevor der Benutzer gesperrt wird. Der Standardwert ist 5. |
| `minRequiredNonalphanumericCharacters` | Die Mindestanzahl nicht alphanumerischer Zeichen, die im Kennwort eines Benutzers angezeigt werden müssen. Dieser Wert muss zwischen 0 und 128 liegen. der Standardwert ist 1. |
| `minRequiredPasswordLength` | Die Mindestanzahl von Zeichen, die in einem Kennwort erforderlich sind. Dieser Wert muss zwischen 0 und 128 liegen. der Standardwert ist 7. |
| `name` | Der Name des registrierten Anbieters. Dieser Wert ist erforderlich. |
| `passwordAttemptWindow` | Die Anzahl der Minuten, während der fehlgeschlagene Anmeldeversuche nachverfolgt werden. Wenn ein Benutzer innerhalb dieses angegebenen Fensters `maxInvalidPasswordAttempts` Zeiten ungültige Anmelde Informationen angibt, werden diese gesperrt. Der Standardwert ist 10. |
| `PasswordFormat` | Das Kenn Wort Speicherformat: `Clear`, `Hashed`oder `Encrypted`. Der Standardwert ist `Hashed`. |
| `passwordStrengthRegularExpression` | Wenn angegeben, wird dieser reguläre Ausdruck zum Auswerten der Stärke des ausgewählten Kennworts des Benutzers verwendet, wenn ein neues Konto erstellt oder das Kennwort geändert wird. Der Standardwert ist eine leere Zeichenfolge. |
| `requiresQuestionAndAnswer` | Gibt an, ob ein Benutzer seine Sicherheitsfrage beim Abrufen oder Zurücksetzen seines Kennworts beantworten muss. Der Standardwert ist `true`sein. |
| `requiresUniqueEmail` | Gibt an, ob alle Benutzerkonten in einer bestimmten Anwendungs Partition über eine eindeutige e-Mail-Adresse verfügen müssen. Der Standardwert ist `true`sein. |
| `type` | Gibt den Typ des Anbieters an. Dieser Wert ist erforderlich. |

**Tabelle 2**: Mitgliedschafts-und `SqlMembershipProvider` Konfigurationseinstellungen

Zusätzlich zu `AspNetSqlMembershipProvider`können andere Mitgliedschafts Anbieter auf Anwendungs Basis registriert werden, indem der `Web.config` Datei ein ähnliches Markup hinzugefügt wird.

> [!NOTE]
> Das Framework für Rollen funktioniert ähnlich: Es gibt einen standardmäßigen registrierten Rollen Anbieter in `machine.config`, und die registrierten Anbieter können in `Web.config`auf Anwendungs Basis angepasst werden. In einem zukünftigen Tutorial werden das Rollen Framework und das dazugehörige Konfigurations Markup ausführlich erläutert.

### <a name="customizing-thesqlmembershipprovidersettings"></a>Anpassen der`SqlMembershipProvider`Einstellungen

Für den Standard `SqlMembershipProvider` (`AspNetSqlMembershipProvider`) ist das `connectionStringName`-Attribut auf `LocalSqlServer`festgelegt. Wie der `AspNetSqlMembershipProvider`-Anbieter wird der Name der Verbindungs Zeichenfolge `LocalSqlServer` in `machine.config`definiert.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-vb/samples/sample2.xml)]

Wie Sie sehen können, definiert diese Verbindungs Zeichenfolge eine SQL 2005 Express Edition-Datenbank unter | DataDirectory | aspnetdb. mdf. Die Zeichenfolge | DataDirectory | wird zur Laufzeit übersetzt, um auf das `~/App_Data/` Verzeichnis zu verweisen, sodass der Daten bankpfad | DataDirectory | aspnetdb. mdf wird in `~/App_Data`/`aspnet.mdf`übersetzt.

Wenn in der `Web.config` Datei der Anwendung keine Mitgliedschafts Anbieter Informationen angegeben wurden, verwendet die Anwendung den standardmäßigen registrierten Mitgliedschafts Anbieter `AspNetSqlMembershipProvider`. Wenn die `~/App_Data/aspnet.mdf` Datenbank nicht vorhanden ist, wird Sie von der ASP.NET-Laufzeit automatisch erstellt und das Anwendungs Dienst Schema hinzugefügt. Wir möchten jedoch nicht die `aspnet.mdf` Datenbank verwenden. Stattdessen möchten wir die `SecurityTutorials.mdf` Datenbank verwenden, die wir in Schritt 2 erstellt haben. Diese Änderung kann auf zwei Arten durchgeführt werden:

- Geben Sie in`Web.config`<strong>einen Wert für den Namen der</strong> <strong>`LocalSqlServer`</strong> <strong>Verbindungs Zeichenfolge an</strong> <strong>.</strong> Wenn Sie den Wert `LocalSqlServer` Verbindungs Zeichenfolgen-Name in `Web.config`überschreiben, können wir den standardmäßigen registrierten Mitgliedschafts Anbieter (`AspNetSqlMembershipProvider`) verwenden und ihn ordnungsgemäß mit der `SecurityTutorials.mdf` Datenbank arbeiten lassen. Diese Vorgehensweise ist in Ordnung, wenn Sie Inhalte mit den von `AspNetSqlMembershipProvider`angegebenen Konfigurationseinstellungen haben. Weitere Informationen zu dieser Technik finden Sie im Blogbeitrag von [Scott Guthrie](https://weblogs.asp.net/scottgu/), [Konfigurieren von ASP.NET 2,0 Anwendungsdienste für die Verwendung von SQL Server 2000 oder SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx).
- <strong>Fügen Sie einen neuen registrierten Anbieter vom Typ</strong> <strong>`SqlMembershipProvider`</strong>hinzu, <strong>und konfigurieren Sie seine</strong> <strong>`connectionStringName`</strong> <strong>Einstellungen so, dass er auf die</strong> <strong>`SecurityTutorials.mdf`</strong> <strong>Datenbank verweist.</strong> Diese Vorgehensweise ist nützlich in Szenarien, in denen andere Konfigurations Eigenschaften zusätzlich zur Daten bankverbindungs Zeichenfolge angepasst werden sollen. In meinen eigenen Projekten verwende ich diesen Ansatz immer aufgrund seiner Flexibilität und Lesbarkeit.

Bevor ein neuer registrierter Anbieter hinzugefügt werden kann, der auf die `SecurityTutorials.mdf` Datenbank verweist, müssen wir zunächst im Abschnitt `<connectionStrings>` in `Web.config`einen entsprechenden Wert für die Verbindungs Zeichenfolge hinzufügen. Das folgende Markup fügt eine neue Verbindungs Zeichenfolge mit dem Namen `SecurityTutorialsConnectionString` hinzu, die auf die SQL Server 2005 Express Edition `SecurityTutorials.mdf` Datenbank im `App_Data` Ordner verweist.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-vb/samples/sample3.xml)]

> [!NOTE]
> Wenn Sie eine Alternative Datenbankdatei verwenden, aktualisieren Sie die Verbindungs Zeichenfolge nach Bedarf. Weitere Informationen zum bilden der richtigen Verbindungs Zeichenfolge finden Sie unter [connectionStrings.com](http://www.connectionstrings.com/).

Fügen Sie als nächstes das folgende Mitgliedschafts Konfigurations Markup zur `Web.config` Datei hinzu. Dieses Markup registriert einen neuen Anbieter mit dem Namen `SecurityTutorialsSqlMembershipProvider`.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-vb/samples/sample4.xml)]

Zusätzlich zum Registrieren des `SecurityTutorialsSqlMembershipProvider` Anbieters definiert das obige Markup das `SecurityTutorialsSqlMembershipProvider` als Standardanbieter (über das `defaultProvider`-Attribut im `<membership>`-Element). Denken Sie daran, dass das Mitgliedschafts Framework mehrere registrierte Anbieter aufweisen kann. Da `AspNetSqlMembershipProvider` als erster Anbieter in `machine.config`registriert ist, fungiert er als Standardanbieter, sofern nicht anders angegeben.

Derzeit verfügt unsere Anwendung über zwei registrierte Anbieter: `AspNetSqlMembershipProvider` und `SecurityTutorialsSqlMembershipProvider`. Vor dem Registrieren des `SecurityTutorialsSqlMembershipProvider` Anbieters konnten wir jedoch alle zuvor registrierten Anbieter durch Hinzufügen eines [`<clear />` Elements](https://msdn.microsoft.com/library/t062y6yc.aspx) direkt vor dem `<add>`-Element löschen. Dadurch wird die `AspNetSqlMembershipProvider` aus der Liste der registrierten Anbieter gelöscht, was bedeutet, dass die `SecurityTutorialsSqlMembershipProvider` der einzige registrierte Mitgliedschafts Anbieter ist. Wenn Sie diesen Ansatz verwenden, müssen wir die `SecurityTutorialsSqlMembershipProvider` nicht als Standardanbieter markieren, da dies der einzige registrierte Mitgliedschafts Anbieter wäre. Weitere Informationen zur Verwendung von `<clear />`finden Sie unter [Verwenden von `<clear />` beim Hinzufügen von Anbietern](https://weblogs.asp.net/scottgu/archive/2006/11/20/common-gotcha-don-t-forget-to-clear-when-adding-providers.aspx).

Beachten Sie, dass die `connectionStringName` Einstellung des `SecurityTutorialsSqlMembershipProvider`auf den soeben hinzugefügten `SecurityTutorialsConnectionString` Verbindungs Zeichenfolgen-Namen verweist und dass seine `applicationName` Einstellung auf den Wert securitytutorials festgelegt wurde. Außerdem wurde die `requiresUniqueEmail` Einstellung auf `true`festgelegt. Alle anderen Konfigurationsoptionen sind identisch mit den Werten in `AspNetSqlMembershipProvider`. Wenn Sie möchten, können Sie an dieser Stelle jederzeit Änderungen an der Konfiguration vornehmen. Beispielsweise können Sie die Kenn Wort Sicherheit erhöhen, indem Sie anstelle eines der beiden nicht-alphanumerischen Zeichen zwei nicht-alphanumerische Zeichen benötigen, oder indem Sie die Kenn Wort Länge auf acht Zeichen anstelle von 7 erhöhen.

> [!NOTE]
> Beachten Sie, dass das Mitgliedschafts Framework ermöglicht, dass ein einzelner Benutzerspeicher über mehrere Anwendungen hinweg partitioniert werden kann. Die `applicationName` Einstellung des Mitgliedschafts Anbieters gibt an, welche Anwendung der Anbieter bei der Arbeit mit dem Benutzerspeicher verwendet. Es ist wichtig, dass Sie explizit einen Wert für die `applicationName` Konfigurationseinstellung festlegen, denn wenn die `applicationName` nicht explizit festgelegt ist, wird Sie zur Laufzeit dem virtuellen Stammpfad der Webanwendung zugewiesen. Dies funktioniert einwandfrei, solange der virtuelle Stammpfad der Anwendung nicht geändert wird. Wenn Sie die Anwendung jedoch in einen anderen Pfad verschieben, ändert sich auch die `applicationName` Einstellung. In diesem Fall wird der Mitgliedschafts Anbieter mit einer anderen Anwendungs Partition arbeiten, als zuvor verwendet wurde. Benutzerkonten, die vor dem Verschieben erstellt wurden, befinden sich in einer anderen Anwendungs Partition, und diese Benutzer können sich nicht mehr bei der Website anmelden. Eine ausführlichere Erläuterung zu diesem Thema finden Sie unter [Festlegen der `applicationName`-Eigenschaft beim Konfigurieren der ASP.NET 2,0-Mitgliedschaft und anderer Anbieter](https://weblogs.asp.net/scottgu/443634).

## <a name="summary"></a>Summary

An dieser Stelle verfügen wir über eine Datenbank mit den konfigurierten Anwendungsdiensten (`SecurityTutorials.mdf`) und haben die Webanwendung so konfiguriert, dass das Mitgliedschafts Framework den `SecurityTutorialsSqlMembershipProvider` Anbieter verwendet, den wir gerade registriert haben. Dieser registrierte Anbieter weist den Typ "`SqlMembershipProvider`" auf und ist dessen `connectionStringName` auf die entsprechende Verbindungs Zeichenfolge (`SecurityTutorialsConnectionString`) und dessen `applicationName` Wert explizit festgelegt.

Wir sind nun bereit, das Mitgliedschafts Framework aus unserer Anwendung zu verwenden. Im nächsten Tutorial wird erläutert, wie neue Benutzerkonten erstellt werden. Im folgenden erfahren Sie, wie Sie die Authentifizierung von Benutzern, die Durchführung einer benutzerbasierten Autorisierung und das Speichern zusätzlicher Benutzer bezogener Informationen in der-Datenbank untersuchen.

Fröhliche Programmierung!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den in diesem Tutorial behandelten Themen finden Sie in den folgenden Ressourcen:

- [Legen Sie die `applicationName`-Eigenschaft beim Konfigurieren der ASP.NET 2,0-Mitgliedschaft und anderer Anbieter immer fest.](https://weblogs.asp.net/scottgu/443634)
- [Konfigurieren von ASP.NET 2,0 Anwendungsdienste für die Verwendung SQL Server 2000 oder SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx)
- [Herunterladen von SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)
- [Untersuchen von ASP.NET 2,0 s Mitgliedschaft, Rollen und profile](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Das `<add>`-Element für Anbieter für die Mitgliedschaft.](https://msdn.microsoft.com/library/whae3t94.aspx)
- [Das `<membership>`-Element](https://msdn.microsoft.com/library/1b9hw62f.aspx)
- [Das `<providers>` Element für die Mitgliedschaft.](https://msdn.microsoft.com/library/6d4936ht.aspx)
- [Verwenden von `<clear />` beim Hinzufügen von Anbietern](https://weblogs.asp.net/scottgu/archive/2006/11/20/common-gotcha-don-t-forget-to-clear-when-adding-providers.aspx)
- [Direktes Arbeiten mit dem `SqlMembershipProvider`](http://aspnet.4guysfromrolla.com/articles/091207-1.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Video Schulung zu Themen in diesem Tutorial

- [Grundlegendes zu ASP.NET-Mitgliedschaften](../../../videos/authentication/understanding-aspnet-memberships.md)
- [Konfigurieren von SQL für die Arbeit mit Mitgliedschaftsschemas](../../../videos/authentication/configuring-sql-to-work-with-membership-schemas.md)
- [Ändern der Mitgliedschaftseinstellungen im Mitgliedschaftsschema](../../../videos/authentication/changing-membership-settings-in-the-default-membership-schema.md)

### <a name="about-the-author"></a>Informationen zum Autor

Scott Mitchell, Autor mehrerer ASP/ASP. net-Bücher und Gründer von 4GuysFromRolla.com, hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist *[Sams Teach Yourself ASP.NET 2,0 in 24 Stunden](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott kann über [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/)erreicht werden.

### <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Lead Reviewer für dieses Tutorial war Alicja Maziarz. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)ablegen.

> [!div class="step-by-step"]
> [Zurück](storing-additional-user-information-cs.md)
> [Weiter](creating-user-accounts-vb.md)
