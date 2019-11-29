---
uid: web-forms/overview/deployment/visual-studio-web-deployment/preparing-databases
title: 'ASP.net-Webbereitstellung mithilfe von Visual Studio: Vorbereiten der Daten Bank Bereitstellung | Microsoft-Dokumentation'
author: tdykstra
description: In dieser tutorialreihe erfahren Sie, wie Sie eine ASP.NET-Webanwendung bereitstellen (veröffentlichen), um Web-Apps oder einen Drittanbieter-Hostinganbieter zu Azure App Service.
ms.author: riande
ms.date: 02/15/2013
ms.assetid: ae4def81-fa37-4883-a13e-d9896cbf6c36
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/preparing-databases
msc.type: authoredcontent
ms.openlocfilehash: cdcb3578725c41e3c801afd54e6d34455bc4b281
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74618536"
---
# <a name="aspnet-web-deployment-using-visual-studio-preparing-for-database-deployment"></a>ASP.net-Webbereitstellung mithilfe von Visual Studio: Vorbereiten der Daten Bank Bereitstellung

von [Tom Dykstra](https://github.com/tdykstra)

[Starter Projekt herunterladen](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> In dieser tutorialreihe erfahren Sie, wie Sie eine ASP.NET-Webanwendung bereitstellen (veröffentlichen), um Web-Apps oder einen Drittanbieter-Hostinganbieter mithilfe von Visual Studio 2012 oder Visual Studio 2010 zu Azure App Service. Weitere Informationen zur Reihe finden Sie [im ersten Tutorial der Reihe](introduction.md).

## <a name="overview"></a>Übersicht über

In diesem Tutorial wird gezeigt, wie Sie das Projekt für die Daten Bank Bereitstellung vorbereiten. Die Datenbankstruktur und einige (nicht alle) Daten in den beiden Datenbanken der Anwendung müssen für Test-, Staging-und Produktionsumgebungen bereitgestellt werden.

In der Regel geben Sie beim Entwickeln einer Anwendung Testdaten in eine Datenbank ein, die Sie nicht auf einer Live Website bereitstellen möchten. Möglicherweise haben Sie aber auch einige Produktionsdaten, die Sie bereitstellen möchten. In diesem Tutorial konfigurieren Sie das Projekt "" der Projekt "" der Projekt Erstellung und bereiten SQL-Skripts vor, damit bei der Bereitstellung die richtigen Daten enthalten sind.

Erinnerung: Wenn Sie eine Fehlermeldung erhalten oder etwas nicht funktioniert, wenn Sie das Tutorial durchlaufen, achten Sie darauf, dass Sie die [Seite Problem](troubleshooting.md)Behandlung überprüfen.

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

Die Beispielanwendung verwendet SQL Server Express localdb. SQL Server Express ist die kostenlose Edition von SQL Server. Sie wird häufig während der Entwicklung verwendet, da Sie auf derselben Datenbank-Engine basiert wie Vollversionen SQL Server. Sie können mit SQL Server Express testen und sicher sein, dass die Anwendung in der Produktion identisch verhält, mit einigen Ausnahmen für Features, die sich zwischen SQL Server Editionen unterscheiden.

Localdb ist ein spezieller Ausführungs Modus SQL Server Express, mit dem Sie Datenbanken als *MDF* -Dateien bearbeiten können. Normalerweise werden localdb-Datenbankdateien im *App-\_Daten* Ordner eines Webprojekts gespeichert. Mit der benutzerinstanzfunktion in SQL Server Express können Sie auch mit *MDF* -Dateien arbeiten, aber die benutzerinstanzfunktion ist veraltet. Daher wird localdb zum Arbeiten mit *MDF* -Dateien empfohlen.

In der Regel wird SQL Server Express nicht für produktionsweb Anwendungen verwendet. Localdb wird insbesondere für den Einsatz in der Produktion mit einer Webanwendung nicht empfohlen, da es nicht für die Verwendung mit IIS konzipiert ist.

In Visual Studio 2012 wird localdb standardmäßig mit Visual Studio installiert. In Visual Studio 2010 und früheren Versionen wird SQL Server Express (ohne localdb) standardmäßig mit Visual Studio installiert. Daher haben Sie es als eine der Voraussetzungen im [ersten Tutorial dieser Reihe](introduction.md)installiert.

Weitere Informationen zu SQL Server Editionen, einschließlich localdb, finden Sie in den folgenden Ressourcen zum [Arbeiten mit SQL Server-Datenbanken](../../../../whitepapers/aspnet-data-access-content-map.md#sqlserver).

## <a name="entity-framework-and-universal-providers"></a>Entity Framework und universelle Anbieter

Für den Datenbankzugriff erfordert die Anwendung der Anwendung "der Anwendung" die folgende Software, die mit der Anwendung bereitgestellt werden muss, da Sie nicht in der .NET Framework enthalten ist:

- [ASP.net-universelle Anbieter](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) (aktiviert das ASP.NET-Mitgliedschaftssystem zur Verwendung von Azure SQL-Datenbank)
- [Entity Framework](https://msdn.microsoft.com/library/gg696172.aspx)

Da diese Software in nuget-Paketen enthalten ist, ist das Projekt bereits so eingerichtet, dass die erforderlichen Assemblys mit dem Projekt bereitgestellt werden. (Die Verknüpfungen verweisen auf die aktuellen Versionen dieser Pakete, die neuer sein können als die in dem Starter-Projekt, das Sie für dieses Tutorial heruntergeladen haben.)

Wenn Sie anstelle von Azure für einen Drittanbieter-Hosting-Anbieter bereitstellen, stellen Sie sicher, dass Sie Entity Framework 5,0 oder höher verwenden. Frühere Versionen von Code First-Migrationen erfordern volle Vertrauenswürdigkeit, und die meisten Hostinganbieter führen Ihre Anwendung mit mittlerer Vertrauenswürdigkeit aus. Weitere Informationen zur mittleren Vertrauenswürdigkeit finden Sie im Tutorial bereitstellen [in IIS als Test Umgebung](deploying-to-iis.md) .

## <a name="configure-code-first-migrations-for-application-database-deployment"></a>Konfigurieren von Code First-Migrationen für die Bereitstellung der Anwendungsdatenbank

Die Anwendungsdatenbank der Datenbank "der Datenbank" wird von Code First verwaltet, und Sie stellen Sie mithilfe von Code First-Migrationen bereit. Eine Übersicht über die Daten Bank Bereitstellung mithilfe von Code First-Migrationen finden Sie [im ersten Tutorial dieser Reihe](introduction.md).

Wenn Sie eine Anwendungsdatenbank bereitstellen, stellen Sie in der Regel nicht einfach nur Ihre Entwicklungs Datenbank mit sämtlichen Daten in der Produktionsumgebung bereit, da viele der darin vorhandenen Daten wahrscheinlich nur zu Testzwecken vorhanden sind. Die Namen der Studenten in einer Testdatenbank sind z. b. fiktiv. Andererseits ist es oft nicht möglich, nur die Datenbankstruktur ohne Daten bereitzustellen. Einige Daten in der Testdatenbank können echte Daten sein und müssen vorhanden sein, wenn die Benutzer die Anwendung verwenden. Beispielsweise kann die Datenbank über eine Tabelle verfügen, die gültige Grade-Werte oder echte Abteilungsnamen enthält.

Um dieses gängige Szenario zu simulieren, konfigurieren Sie eine Code First-Migrationen `Seed` Methode, die nur die Daten in die Datenbank einfügt, die in der Produktionsumgebung vorhanden sein sollen. Diese `Seed` Methode sollte keine Testdaten einfügen, da Sie in der Produktionsumgebung ausgeführt wird, nachdem Code First die Datenbank in der Produktionsumgebung erstellt hat.

In früheren Versionen von Code First vor der Veröffentlichung von Migrationen war es üblich, dass `Seed` Methoden auch Testdaten einfügen, da bei jeder Modell Änderung während der Entwicklung die Datenbank vollständig gelöscht und neu erstellt werden musste. Mit Code First-Migrationen werden Testdaten nach der Daten Bank Änderung beibehalten, sodass das Einschließen von Testdaten in die `Seed` Methode nicht erforderlich ist. Das Projekt, das Sie heruntergeladen haben, verwendet die-Methode, mit der alle Daten in der `Seed`-Methode einer Initialisiererklasse eingeschlossen werden. In diesem Tutorial deaktivieren Sie die Initialisiererklasse und aktivieren Migrationen. Anschließend aktualisieren Sie die `Seed`-Methode in der migrationskonfigurationsklasse, sodass nur Daten eingefügt werden, die in die Produktion eingefügt werden sollen.

Das folgende Diagramm veranschaulicht das Schema der Anwendungsdatenbank:

[![School_database_diagram](preparing-databases/_static/image2.png)](preparing-databases/_static/image1.png)

In diesen Tutorials wird davon ausgegangen, dass die `Student`-und `Enrollment` Tabellen bei der ersten Bereitstellung der Website leer sein sollten. Die anderen Tabellen enthalten Daten, die vorab geladen werden müssen, wenn die Anwendung online geschaltet wird.

### <a name="disable-the-initializer"></a>Initialisierer deaktivieren

Da Sie Code First-Migrationen verwenden, müssen Sie den `DropCreateDatabaseIfModelChanges` Code First-Initialisierer nicht verwenden. Der Code für diesen Initialisierer befindet sich in der *SchoolInitializer.cs* -Datei im Projekt condesouniversity. DAL. Eine Einstellung im `appSettings`-Element der Datei *Web. config* bewirkt, dass dieser Initialisierer ausgeführt wird, wenn die Anwendung zum ersten Mal versucht, auf die Datenbank zuzugreifen:

[!code-xml[Main](preparing-databases/samples/sample1.xml?highlight=3)]

Öffnen Sie die Datei *Web. config* der Anwendung, und entfernen Sie das `add` Element, das die Code First Initialisiererklasse angibt, oder kommentieren Sie es aus. Das `appSettings`-Element sieht nun wie folgt aus:

[!code-xml[Main](preparing-databases/samples/sample2.xml)]

> [!NOTE]
> Eine andere Möglichkeit, eine Initialisiererklasse anzugeben, ist das Aufrufen von `Database.SetInitializer` in der `Application_Start`-Methode in der Datei " *Global. asax* ". Wenn Sie Migrationen in einem Projekt aktivieren, das diese Methode verwendet, um den Initialisierer anzugeben, entfernen Sie diese Codezeile.

> [!NOTE]
> Wenn Sie Visual Studio 2013 verwenden, fügen Sie die folgenden Schritte zwischen den Schritten 2 und 3 hinzu: (a) in der PMC geben Sie "Update-Package EntityFramework-Version 6.1.1" ein, um die aktuelle Version von EF zu erhalten. Erstellen Sie dann (b) das Projekt, um eine Liste der Buildfehler zu erhalten, und beheben Sie Sie. Löschen Sie using-Anweisungen für Namespaces, die nicht mehr vorhanden sind, klicken Sie mit der rechten Maustaste, und klicken Sie auf auflösen, um die erforderlichen using-Anweisungen hinzuzufügen und die Vorkommen von System. Data. EntityState in System. Data. Entity. EntityState zu ändern.

### <a name="enable-code-first-migrations"></a>Aktivieren von Code First-Migrationen

1. Stellen Sie sicher, dass das Projekt conjesouniversity (nicht conjesouniversity. DAL) als Startprojekt festgelegt ist. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt conjesouniversity, und wählen Sie **als Startprojekt festlegen**aus. Code First-Migrationen sucht im Startprojekt nach der Daten bankverbindungs Zeichenfolge.
2. Klicken Sie **im Menü** Extras auf **nuget-Paket-Manager** > Paket-Manager- **Konsole**.

    ![Selecting_Package_Manager_Console](preparing-databases/_static/image3.png)
3. Wählen Sie oben im Konsolenfenster des **Paket-Managers** die Option condesouniversity. dal als Standard Projekt aus, und geben Sie dann an der `PM>` Eingabeaufforderung "Enable-Migrationen" ein.

    ![Enable-Migrations-Befehl](preparing-databases/_static/image4.png)

    (Wenn Sie eine Fehlermeldung erhalten, die besagt, dass der Befehl *enable-Migrations* nicht erkannt wird, geben Sie den Befehl *Update-Package EntityFramework-REINSTALL* ein, und versuchen Sie es noch mal.)

    Dieser Befehl erstellt einen *Migrations* Ordner im Projekt condesouniversity. dal und fügt diesen Ordner zwei Dateien hinzu: eine *Configuration.cs* -Datei, die Sie zum Konfigurieren von Migrationen verwenden können, und eine *InitialCreate.cs* -Datei für die erste Migration, mit der die Datenbank erstellt wird.

    ![Migrations Ordner](preparing-databases/_static/image5.png)

    Sie haben das DAL-Projekt in der Dropdown Liste **Standard Projekt** der **Paket-Manager-Konsole** ausgewählt, da der `enable-migrations`-Befehl in dem Projekt ausgeführt werden muss, das die Code First Context-Klasse enthält. Wenn sich diese Klasse in einem Klassen Bibliotheksprojekt befindet, sucht Code First-Migrationen im Startprojekt der Projekt Mappe nach der Datenbank-Verbindungs Zeichenfolge. In der Lösung conjesouniversity wurde das Webprojekt als Startprojekt festgelegt. Wenn Sie das Projekt, das die Verbindungs Zeichenfolge enthält, nicht als Startprojekt in Visual Studio festlegen möchten, können Sie das Startprojekt im PowerShell-Befehl angeben. Geben Sie den Befehl `get-help enable-migrations`ein, um die Befehlssyntax anzuzeigen.

    Der Befehl `enable-migrations` hat die erste Migration automatisch erstellt, da die Datenbank bereits vorhanden ist. Eine Alternative besteht darin, dass Migrationen die Datenbank erstellen. Verwenden Sie hierzu **Server-Explorer** oder **SQL Server-Objekt-Explorer** , um die Conto souniversity-Datenbank vor dem Aktivieren von Migrationen zu löschen. Nachdem Sie die Migrationen aktiviert haben, erstellen Sie die erste Migration manuell, indem Sie den Befehl "Add-Migration InitialCreate" eingeben. Sie können dann die Datenbank erstellen, indem Sie den Befehl "Update-Database" eingeben.

### <a name="set-up-the-seed-method"></a>Einrichten der Seed-Methode

In diesem Tutorial fügen Sie dem `Seed`-Methode der Code First-Migrationen `Configuration`-Klasse einen Code hinzu. Code First-Migrationen nach jeder Migration die `Seed`-Methode aufruft.

Da die `Seed`-Methode nach jeder Migration ausgeführt wird, sind nach der ersten Migration bereits Daten in den Tabellen vorhanden. Um dieses Problem zu behandeln, verwenden Sie die `AddOrUpdate`-Methode, um bereits eingefügte Zeilen zu aktualisieren, oder fügen Sie Sie ein, wenn Sie noch nicht vorhanden sind. Die `AddOrUpdate`-Methode ist möglicherweise nicht die beste Wahl für Ihr Szenario. Weitere Informationen finden Sie im Blog von Julie Lerman [mit der EF 4,3 addorupdate-Methode](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) .

1. Öffnen Sie die Datei *Configuration.cs* , und ersetzen Sie die Kommentare in der `Seed`-Methode durch den folgenden Code:

    [!code-csharp[Main](preparing-databases/samples/sample3.cs)]
2. Die Verweise auf `List` verfügen über rote Wellenlinien, da Sie noch keine `using`-Anweisung für Ihren Namespace haben. Klicken Sie mit der rechten Maustaste auf eine der Instanzen `List`, klicken Sie auf **Auflösen**, und klicken Sie dann auf **System. Collections. Generic**.

    ![Mit using-Anweisung auflösen](preparing-databases/_static/image6.png)

    Diese Menü Auswahl fügt dem `using`-Anweisungen am Anfang der Datei den folgenden Code hinzu.

    [!code-csharp[Main](preparing-databases/samples/sample4.cs)]
3. Drücken Sie STRG + UMSCHALT + B, um das Projekt zu erstellen.

Das Projekt ist nun bereit für die Bereitstellung der Datenbank *conjesouniversity* . Wenn Sie die Anwendung zum ersten Mal ausführen und zu einer Seite navigieren, die auf die Datenbank zugreift, wird Code First die Datenbank erstellen und diese `Seed` Methode ausführen.

> [!NOTE]
> Das Hinzufügen von Code zur `Seed`-Methode ist eine von vielen Möglichkeiten, dass Sie festgelegte Daten in die Datenbank einfügen können. Eine Alternative besteht darin, den Methoden `Up` und `Down` der einzelnen Migrations Klassen Code hinzuzufügen. Die Methoden `Up` und `Down` enthalten Code, mit dem Daten Bank Änderungen implementiert werden. Beispiele hierfür finden Sie im Tutorial bereitstellen [eines Datenbankupdates](deploying-a-database-update.md) .
> 
> Sie können auch Code schreiben, der SQL-Anweisungen ausführt, indem Sie die `Sql`-Methode verwenden. Wenn Sie z. b. der Abteilungs Tabelle eine Budget Spalte hinzufügen und alle abteilungsbudgets im Rahmen einer Migration auf $1.000,00 initialisieren möchten, können Sie die folgende Codezeile zur `Up`-Methode für diese Migration hinzufügen:
> 
> `Sql("UPDATE Department SET Budget = 1000");`

## <a name="create-scripts-for-membership-database-deployment"></a>Erstellen von Skripts für die Daten Bank Bereitstellung

Die Anwendung der Anwendung "ASP.net" verwendet das-Mitgliedschaftssystem und die Formular Authentifizierung, um Benutzer zu authentifizieren und zu autorisieren. Auf die Seite zum **Aktualisieren von Gutschriften** kann nur von Benutzern zugegriffen werden, die sich in der Administrator Rolle befinden.

Führen Sie die Anwendung aus, und klicken Sie auf **Kurse**und dann auf **Guthaben aktualisieren**.

![Klicken Sie auf Guthaben aktualisieren](preparing-databases/_static/image7.png)

Die **Anmelde** Seite wird angezeigt, da die Seite " **Guthaben aktualisieren** " Administratorrechte erfordert.

Geben Sie *Admin* als Benutzername und *devpwd* als Kennwort ein, und klicken Sie auf **Anmelden**.

![Anmeldeseite](preparing-databases/_static/image8.png)

Die Seite " **Guthaben aktualisieren** " wird angezeigt.

![Seite "Guthaben aktualisieren"](preparing-databases/_static/image9.png)

Benutzer-und Rollen Informationen befinden sich in der *ASPNET-contosouniversity-* Datenbank, die durch die **DefaultConnection** -Verbindungs Zeichenfolge in der Datei " *Web. config* " angegeben wird.

Diese Datenbank wird nicht von Entity Framework Code First verwaltet, sodass Sie keine Migrationen verwenden können, um Sie bereitzustellen. Sie verwenden den dbdacfx-Anbieter, um das Datenbankschema bereitzustellen, und Sie konfigurieren das Veröffentlichungs Profil, um ein Skript auszuführen, mit dem anfängliche Daten in Datenbanktabellen eingefügt werden.

> [!NOTE]
> Ein neues ASP.NET-Mitgliedschaftssystem (jetzt mit dem Namen "ASP.net Identity" bezeichnet) wurde mit Visual Studio 2013 eingeführt. Das neue System ermöglicht es Ihnen, sowohl Anwendungs-als auch Mitgliedschafts Tabellen in derselben Datenbank beizubehalten, und Sie können Code First-Migrationen zum Bereitstellen beider Tabellen verwenden. Die Beispielanwendung verwendet das frühere ASP.NET-Mitgliedschaftssystem, das nicht mithilfe von Code First-Migrationen bereitgestellt werden kann. Die Verfahren für die Bereitstellung dieser Mitgliedschafts Datenbank gelten auch für alle anderen Szenarien, in denen Ihre Anwendung eine SQL Server Datenbank bereitstellen muss, die nicht durch Entity Framework Code First erstellt wurde.

Hier sind die Daten in der Produktion, die Sie in der Entwicklung haben, in der Regel nicht. Wenn Sie einen Standort zum ersten Mal bereitstellen, ist es üblich, die meisten oder alle Benutzerkonten auszuschließen, die Sie zu Testzwecken erstellen. Daher verfügt das heruntergeladene Projekt über zwei Mitgliedschafts Datenbanken: *ASPNET-ContosoUniversity. mdf* mit Entwicklungs Benutzer und *ASPNET-ContosoUniversity-Prod. mdf* mit Produktions Benutzern. In diesem Tutorial sind die Benutzernamen in beiden Datenbanken identisch: *Admin* und *nonadmin*. Beide Benutzer haben das Kennwort *devpwd* in der Entwicklungs Datenbank und *prodpwd* in der Produktionsdatenbank.

Die Entwicklungs Benutzer werden für die Testumgebung und die Produktions Benutzer für Staging und Produktion bereitgestellt. Zu diesem Zweck erstellen Sie in diesem Tutorial zwei SQL-Skripts: einen für die Entwicklung und einen für die Produktion, und in späteren Tutorials konfigurieren Sie den Veröffentlichungsprozess, um ihn auszuführen.

> [!NOTE]
> Die Mitgliedschafts Datenbank speichert einen Hashwert für Konto Kennwörter. Um Konten von einem Computer auf einen anderen Computer bereitzustellen, müssen Sie sicherstellen, dass von den Hash Routinen keine anderen Hashes auf dem Zielserver generiert werden als auf dem Quellcomputer. Sie generieren die gleichen Hashes, wenn Sie die ASP.net-universelle Anbieter verwenden, sofern Sie den Standard Algorithmus nicht ändern. Der Standard Algorithmus ist HMACSHA256 und wird im **Validierungs** Attribut des **[machineKey](https://msdn.microsoft.com/library/system.web.configuration.machinekeysection.aspx)** -Elements in der Datei Web. config angegeben.

Sie können Daten Bereitstellungs Skripts manuell erstellen, indem Sie SQL Server Management Studio (SSMS) verwenden oder ein Drittanbieter Tool verwenden. In diesem verbleibenden Teil dieses Tutorials erfahren Sie, wie Sie in SSMS Vorgehen, aber wenn Sie SSMS nicht installieren und verwenden möchten, können Sie die Skripts aus der abgeschlossenen Version des Projekts erhalten und den Abschnitt überspringen, in dem Sie Sie im Projektmappenordner speichern.

Um SSMS zu installieren, installieren Sie es aus dem [Download Center: Microsoft SQL Server 2012 Express](https://www.microsoft.com/download/details.aspx?id=29062) , indem Sie auf [enu\x64\sqlmanagementstudio\_x64\_ENU. exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x64/SQLManagementStudio_x64_ENU.exe) oder [enu\x86\sqlmanagementstudio\_x86\_ENU. exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x86/SQLManagementStudio_x86_ENU.exe)klicken. Wenn Sie für Ihr System die falsche Option auswählen, kann die Installation nicht ausgeführt werden, und Sie können die andere testen.

(Beachten Sie, dass es sich um einen Download von 600 Megabyte handelt. Die Installation kann einige Zeit in Anspruch nehmen, und es ist ein Neustart des Computers erforderlich.)

Klicken Sie auf der ersten Seite des SQL Server-Installations Centers auf **neu SQL Server eigenständige Installation, oder fügen Sie einer vorhandenen Installation Features hinzu**, und befolgen Sie die Anweisungen, und akzeptieren Sie die Standardoptionen.

### <a name="create-the-development-database-script"></a>Erstellen des Skripts für die Entwicklungs Datenbank

1. Führen Sie SSMS aus.
2. Geben Sie im Dialogfeld **Verbindung mit Server herstellen** *(localdb) \v11.0* als **Server Name**ein, lassen Sie die **Authentifizierung** auf **Windows-Authentifizierung**fest, und klicken Sie dann auf **verbinden**.

    ![SSMS-Verbindung mit Server herstellen](preparing-databases/_static/image10.png)
3. Erweitern Sie im Fenster **Objekt-Explorer** den Eintrag **Datenbanken**, klicken Sie mit der rechten Maustaste auf **ASPNET-contosouniversity**, klicken Sie auf **Tasks**und dann auf **Skripts generieren**.

    ![SSMS-Skripts generieren](preparing-databases/_static/image11.png)
4. Klicken Sie im Dialogfeld **Skripts generieren und veröffentlichen** auf Skript Erstellungs **Optionen festlegen**.

    Sie können den Schritt " **Objekte auswählen** " überspringen, da der Standardwert Skripterstellung für **gesamte Datenbank und alle Datenbankobjekte** ist, und das ist der gewünschte Wert.
5. Klicken Sie auf **Erweitert**.

    ![SSMS-Skript Erstellungs Optionen](preparing-databases/_static/image12.png)
6. Führen Sie im Dialogfeld **Erweiterte Skript** Erstellungs Optionen einen Bildlauf nach unten zu den **Datentypen für das Skript**aus, und klicken Sie in der Dropdown Liste auf die Option **nur Daten** .
7. **Skript ändern verwendet Datenbank** in **false**. USE-Anweisungen sind für Azure SQL-Datenbank nicht gültig und müssen nicht für die Bereitstellung in der Testumgebung SQL Server Express werden.

    ![Nur SSMS-Skript Daten, keine USE-Anweisung](preparing-databases/_static/image13.png)
8. Klicken Sie auf **OK**.
9. Im Dialogfeld **Skripts generieren und veröffentlichen** wird im Feld **Dateiname** angegeben, wo das Skript erstellt wird. Ändern Sie den Pfad zu Ihrem Projektmappenordner (dem Ordner mit der Datei condesouniversity. sln) und dem Dateinamen in *ASPNET-Data-dev. SQL*.
10. Klicken Sie auf **weiter** , um zur Registerkarte **Zusammenfassung** zu wechseln, und klicken Sie erneut auf **weiter** , um das Skript zu erstellen.

    ![SSMS-Skript erstellt](preparing-databases/_static/image14.png)
11. Klicken Sie auf **Fertig stellen**.

### <a name="create-the-production-database-script"></a>Erstellen des Skripts für die Produktionsdatenbank

Da Sie das Projekt nicht mit der Produktionsdatenbank ausgeführt haben, ist es noch nicht an die localdb-Instanz angefügt. Daher müssen Sie zuerst die Datenbank anfügen.

1. Klicken Sie im **Objekt-Explorer**SSMS mit der rechten Maustaste auf **Datenbanken** , und klicken Sie auf **Anfügen**

    ![SSMS anfügen](preparing-databases/_static/image15.png)
2. Klicken Sie im Dialogfeld **Datenbanken** **Anfügen auf Hinzufügen** , und navigieren Sie dann zu der Datei *ASPNET-ContosoUniversity-Prod. mdf* im Ordner *App\_Data* .

     ![Hinzufügen einer MDF-Datei zum Anfügen durch SSMS](preparing-databases/_static/image16.png)
3. Klicken Sie auf **OK**.
4. Befolgen Sie das Verfahren, das Sie zuvor zum Erstellen eines Skripts für die Produktions Datei verwendet haben. Benennen Sie die Skriptdatei *ASPNET-Data-Prod. SQL*.

## <a name="summary"></a>Summary

Beide Datenbanken können jetzt bereitgestellt werden, und Sie verfügen über zwei Skripts zur Datenbereitstellung in Ihrem Projektmappenordner.

![Daten Bereitstellungs Skripts](preparing-databases/_static/image17.png)

Im folgenden Tutorial konfigurieren Sie die Projekteinstellungen, die sich auf die Bereitstellung auswirken, und Sie richten automatische *Web. config* -Datei Transformationen für Einstellungen ein, die in der bereitgestellten Anwendung unterschiedlich sein müssen.

## <a name="more-information"></a>Weitere Informationen

Weitere Informationen zu nuget finden Sie unter [Verwalten von Projekt Bibliotheken mit nuget](https://msdn.microsoft.com/magazine/hh547106.aspx) und [nuget-Dokumentation](http://docs.nuget.org/docs/start-here/overview). Wenn Sie nuget nicht verwenden möchten, müssen Sie erfahren, wie Sie ein nuget-Paket analysieren, um zu bestimmen, was es bei der Installation durchführt. (Beispielsweise können Sie *Web. config* -Transformationen konfigurieren, PowerShell-Skripts für die Lauf Zeit Erstellung konfigurieren usw.) Weitere Informationen zur Funktionsweise von nuget finden Sie unter [Erstellen und Veröffentlichen von Paketen](http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-package) und [Konfigurationsdateien und Quell Code Transformationen](http://docs.nuget.org/docs/creating-packages/configuration-file-and-source-code-transformations).

> [!div class="step-by-step"]
> [Zurück](introduction.md)
> [Weiter](web-config-transformations.md)
