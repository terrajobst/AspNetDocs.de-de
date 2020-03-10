---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12
title: 'Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mithilfe von Visual Studio oder Visual Web Developer: Bereitstellen von SQL Server Compact Datenbanken-2 von 12 | Microsoft-Dokumentation'
author: tdykstra
description: In dieser Reihe von Tutorials wird gezeigt, wie Sie ein ASP.NET-Webanwendungs Projekt bereitstellen (veröffentlichen), das eine SQL Server Compact-Datenbank mit Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: c3c76516-4c48-4153-bd03-d70e3a3edbb0
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12
msc.type: authoredcontent
ms.openlocfilehash: 56ceabc79947967846d342354fd033510be5f05a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78458253"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-sql-server-compact-databases---2-of-12"></a>Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mithilfe von Visual Studio oder Visual Web Developer: Bereitstellen von SQL Server Compact Datenbanken-2 von 12

von [Tom Dykstra](https://github.com/tdykstra)

[Starter Projekt herunterladen](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> In dieser Reihe von Tutorials erfahren Sie, wie Sie ein ASP.NET-Webanwendungs Projekt, das eine SQL Server Compact Datenbank enthält, mithilfe von Visual Studio 2012 RC oder Visual Studio Express 2012 RC für das Web bereitstellen (veröffentlichen). Sie können auch Visual Studio 2010 verwenden, wenn Sie das Webveröffentlichungs Update installieren. Eine Einführung in die Reihe finden Sie [im ersten Tutorial der Reihe](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Ein Tutorial, das nach der RC-Version von Visual Studio 2012 eingeführte Bereitstellungs Funktionen zeigt, zeigt, wie SQL Server Editionen außer SQL Server Compact bereitgestellt werden, und zeigt, wie Sie die Bereitstellung für Azure App Service Web-Apps ausführen. Weitere Informationen finden Sie unter [ASP.net Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Übersicht

In diesem Tutorial wird gezeigt, wie zwei SQL Server Compact-Datenbanken und die Datenbank-Engine für die Bereitstellung eingerichtet werden.

Für den Datenbankzugriff erfordert die Anwendung der Anwendung "der Anwendung" die folgende Software, die mit der Anwendung bereitgestellt werden muss, da Sie nicht in der .NET Framework enthalten ist:

- [SQL Server Compact](https://www.microsoft.com/sqlserver/en/us/editions/compact.aspx) (die Datenbank-Engine).
- [ASP.net-universelle Anbieter](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) (die das ASP.NET-Mitgliedschaftssystem zur Verwendung SQL Server Compact verwenden können)
- [Entity Framework 5,0](https://msdn.microsoft.com/library/gg696172(d=lightweight,v=vs.103).aspx)(Code First mit Migrationen).

Die Datenbankstruktur und einige (nicht alle) Daten in den beiden Datenbanken der Anwendung müssen ebenfalls bereitgestellt werden. In der Regel geben Sie beim Entwickeln einer Anwendung Testdaten in eine Datenbank ein, die Sie nicht auf einer Live Website bereitstellen möchten. Sie können jedoch auch einige Produktionsdaten eingeben, die Sie bereitstellen möchten. In diesem Tutorial konfigurieren Sie das Projekt "Projekt" der Configuration Manager-Datenbank so, dass die erforderliche Software und die richtigen Daten bei der Bereitstellung von eingeschlossen werden.

Erinnerung: Wenn Sie eine Fehlermeldung erhalten oder etwas nicht funktioniert, wenn Sie das Tutorial durchlaufen, achten Sie darauf, dass Sie die [Seite Problem](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)Behandlung überprüfen.

## <a name="sql-server-compact-versus-sql-server-express"></a>SQL Server Compact im Vergleich SQL Server Express

Die Beispielanwendung verwendet SQL Server Compact 4,0. Diese Datenbank-Engine ist eine relativ neue Option für Websites. frühere Versionen von SQL Server Compact funktionieren in einer Webhostingumgebung nicht. SQL Server Compact bietet einige Vorteile im Vergleich zum gängigeren Szenario der Entwicklung mit SQL Server Express und der Bereitstellung in voll SQL Server. Abhängig vom ausgewählten Hostinganbieter können SQL Server Compact günstiger bereitgestellt werden, da einige Anbieter zusätzliche Kosten für die Unterstützung einer vollständigen SQL Server Datenbank berechnen. Es fallen keine zusätzlichen Kosten für SQL Server Compact an, da Sie die Datenbank-Engine selbst als Teil Ihrer Webanwendung bereitstellen können.

Beachten Sie jedoch auch die Einschränkungen. SQL Server Compact unterstützt keine gespeicherten Prozeduren, Trigger, Sichten oder Replikation. (Eine komplette Liste der SQL Server Features, die von SQL Server Compact nicht unterstützt werden, finden Sie [unter Unterschiede zwischen SQL Server Compact und SQL Server](https://msdn.microsoft.com/library/bb896140.aspx).) Außerdem funktionieren einige der Tools, die Sie verwenden können, um Schemas und Daten in SQL Server Express und SQL Server Datenbanken zu bearbeiten, nicht mit SQL Server Compact. Beispielsweise können Sie nicht SQL Server Management Studio oder SQL Server Data Tools in Visual Studio mit SQL Server Compact-Datenbanken verwenden. Sie haben auch andere Optionen für die Arbeit mit SQL Server Compact-Datenbanken:

- Sie können Server-Explorer in Visual Studio verwenden, das eingeschränkte Funktionen zur Datenbankbearbeitung für SQL Server Compact bietet.
- Sie können das Feature zur Datenbankbearbeitung von [webmatrix](https://www.microsoft.com/web/webmatrix/)verwenden, das über mehr Funktionen als Server-Explorer verfügt.
- Sie können mit einem relativ umfassenden Funktionsumfang von Drittanbieter-oder Open-Source-Tools, wie z. b. die [SQL Server Compact Toolbox](https://github.com/ErikEJ/SqlCeToolbox) und das [SQL Compact-Daten-und Schema Skript Hilfsprogramm](https://github.com/ErikEJ/SqlCeToolbox)
- Sie können Ihre eigenen DDL-Skripts (Data Definition Language) schreiben und ausführen, um das Datenbankschema zu bearbeiten.

Sie können mit SQL Server Compact beginnen und später ein Upgrade durchführen, wenn sich Ihre Anforderungen weiterentwickeln. Spätere Tutorials in dieser Reihe veranschaulichen, wie Sie von SQL Server Compact zu SQL Server Express und SQL Server migrieren. Wenn Sie jedoch eine neue Anwendung erstellen und erwarten, dass Sie in naher Zukunft SQL Server benötigen, ist es wahrscheinlich am besten, mit SQL Server oder SQL Server Express zu beginnen.

## <a name="configuring-the-sql-server-compact-database-engine-for-deployment"></a>Konfigurieren des SQL Server Compact Datenbank-Engine für die Bereitstellung

Die erforderliche Software für den Datenzugriff in der Anwendung "" der Anwendung "" der Anwendung "" der Anwendung "Anwendung" wurde hinzugefügt.

- [Sqlservercompact](http://nuget.org/List/Packages/SqlServerCompact)
- [System. Web. Providers](http://nuget.org/List/Packages/System.Web.Providers) (universelle ASP.NET-Anbieter)
- [EntityFramework](http://nuget.org/List/Packages/EntityFramework)
- [EntityFramework. sqlservercompact](http://nuget.org/List/Packages/EntityFramework.sqlservercompact)

Die Verknüpfungen verweisen auf die aktuellen Versionen dieser Pakete, die neuer sein können als die in dem Starter-Projekt, das Sie für dieses Tutorial heruntergeladen haben. Stellen Sie für die Bereitstellung für einen Hostinganbieter sicher, dass Sie Entity Framework 5,0 oder höher verwenden. Frühere Versionen von Code First-Migrationen erfordern volle Vertrauenswürdigkeit, und bei vielen Hostinganbietern wird Ihre Anwendung mit mittlerer Vertrauenswürdigkeit ausgeführt. Weitere Informationen zur mittleren Vertrauenswürdigkeit finden Sie im Tutorial bereitstellen [in IIS als Test Umgebung](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) .

Die nuget-Paketinstallation übernimmt in der Regel alles, was Sie benötigen, um diese Software mit der Anwendung bereitzustellen. In einigen Fällen umfasst dies Aufgaben wie das Ändern der Datei "Web. config" und das Hinzufügen von PowerShell-Skripts, die ausgeführt werden, wenn Sie die Projekt Mappe erstellen. **Wenn Sie die Unterstützung für diese Features (z. b. SQL Server Compact und Entity Framework) ohne nuget hinzufügen möchten, sollten Sie sicherstellen, dass Sie wissen, welche nuget-Paketinstallation durchführt, damit Sie die gleiche Arbeit manuell ausführen können.**

Es gibt eine Ausnahme, bei der nuget nicht alles erledigt, was Sie tun müssen, um eine erfolgreiche Bereitstellung sicherzustellen. Das nuget-Paket sqlservercompact fügt dem Projekt ein Postbuildskript hinzu, das die systemeigenen Assemblys in *x86* -und *amd64* -Unterordner unter dem Projektordner " *bin* " kopiert. das Skript enthält jedoch diese Ordner nicht im Projekt. Dies hat zur Folge, dass Web deploy diese nicht auf die Zielwebsite kopiert, es sei denn, Sie fügen Sie manuell in das Projekt ein. (Dieses Verhalten ergibt sich aus der Standard Bereitstellungs Konfiguration. eine weitere Option, die Sie in diesen Tutorials nicht verwenden, besteht darin, die Einstellung zu ändern, die dieses Verhalten steuert. Die Einstellung, die Sie ändern können, sind **nur Dateien, die zum Ausführen der Anwendung** im Fenster "Projekteigenschaften **" auf der** Registerkarte " **packen/veröffentlichen** " im Fenster " **Projekteigenschaften** " benötigt werden. Das Ändern dieser Einstellung wird im Allgemeinen nicht empfohlen, da dies dazu führen kann, dass viele weitere Dateien in der Produktionsumgebung bereitgestellt werden, als Sie benötigt werden. Weitere Informationen zu den Alternativen finden Sie im Tutorial [Konfigurieren von Projekteigenschaften](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md) .)

Erstellen Sie das Projekt, und klicken Sie dann in **Projektmappen-Explorer** auf **alle Dateien anzeigen** , sofern Sie dies nicht bereits getan haben. Möglicherweise müssen Sie auch auf **Aktualisieren**klicken.

![Solution_Explorer_Show_All_Files](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image1.png)

Erweitern Sie den Ordner **bin** , um die Ordner **amd64** und **x86** anzuzeigen, und wählen Sie dann diese Ordner aus, klicken Sie mit der rechten Maustaste, und wählen Sie **in Projekt einschließen aus**.

![amd64_and_x86_in_Solution_Explorer.png](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image2.png)

Die Ordner Symbole werden geändert, um anzuzeigen, dass der Ordner im Projekt enthalten ist.

![Solution_Explorer_amd64_included.png](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image3.png)

## <a name="configuring-code-first-migrations-for-application-database-deployment"></a>Konfigurieren von Code First-Migrationen für die Bereitstellung der Anwendungsdatenbank

Wenn Sie eine Anwendungsdatenbank bereitstellen, stellen Sie in der Regel nicht einfach nur Ihre Entwicklungs Datenbank mit sämtlichen Daten in der Produktionsumgebung bereit, da viele der darin vorhandenen Daten wahrscheinlich nur zu Testzwecken vorhanden sind. Die Namen der Studenten in einer Testdatenbank sind z. b. fiktiv. Andererseits ist es oft nicht möglich, nur die Datenbankstruktur ohne Daten bereitzustellen. Einige Daten in der Testdatenbank können echte Daten sein und müssen vorhanden sein, wenn die Benutzer die Anwendung verwenden. Beispielsweise kann die Datenbank über eine Tabelle verfügen, die gültige Grade-Werte oder echte Abteilungsnamen enthält.

Um dieses gängige Szenario zu simulieren, konfigurieren Sie eine Code First-Migrationen Seed-Methode, die nur die Daten in die Datenbank einfügt, die in der Produktionsumgebung vorhanden sein sollen. Diese Seed-Methode fügt keine Testdaten ein, da Sie in der Produktion ausgeführt wird, nachdem Code First die Datenbank in der Produktionsumgebung erstellt hat.

In früheren Versionen von Code First vor der Veröffentlichung von Migrationen üblich, dass auch Seed-Methoden Testdaten einfügen, da bei jeder Modell Änderung während der Entwicklung die Datenbank vollständig gelöscht und neu erstellt werden musste. Mit Code First-Migrationen werden Testdaten nach der Daten Bank Änderung beibehalten, sodass das Einschließen von Testdaten in die Seed-Methode nicht erforderlich ist. Das Projekt, das Sie heruntergeladen haben, verwendet die Methode vor der Migration, mit der alle Daten in der Seed-Methode einer Initialisiererklasse eingeschlossen werden. In diesem Tutorial deaktivieren Sie die Initialisiererklasse und aktivieren Migrationen. Anschließend aktualisieren Sie die Seed-Methode in der migrationskonfigurationsklasse, sodass nur Daten eingefügt werden, die in die Produktion eingefügt werden sollen.

Das folgende Diagramm veranschaulicht das Schema der Anwendungsdatenbank:

[![School_database_diagram](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image4.png)

In diesen Tutorials wird davon ausgegangen, dass die `Student`-und `Enrollment` Tabellen bei der ersten Bereitstellung der Website leer sein sollten. Die anderen Tabellen enthalten Daten, die vorab geladen werden müssen, wenn die Anwendung online geschaltet wird.

Da Sie Code First-Migrationen verwenden, müssen Sie den " **dropkreatedatabaseifmodelchanges** "-Code First Initialisierer nicht mehr verwenden. Der Code für diesen Initialisierer befindet sich in der SchoolInitializer.cs-Datei im Projekt condesouniversity. DAL. Eine Einstellung im **appSettings** -Element der Datei Web. config bewirkt, dass dieser Initialisierer ausgeführt wird, wenn die Anwendung zum ersten Mal versucht, auf die Datenbank zuzugreifen:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample1.xml?highlight=3)]

Öffnen Sie die Datei Web. config der Anwendung, und entfernen Sie das Element, das die Code First Initialisiererklasse angibt, aus dem appSettings-Element. Das appSettings-Element sieht nun wie folgt aus:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample2.xml)]

> [!NOTE]
> Eine andere Möglichkeit, eine Initialisiererklasse anzugeben, ist das Aufrufen von `Database.SetInitializer` in der `Application_Start`-Methode in der Datei " *Global. asax* ". Wenn Sie Migrationen in einem Projekt aktivieren, das diese Methode verwendet, um den Initialisierer anzugeben, entfernen Sie diese Codezeile.

Aktivieren Sie als nächstes Code First-Migrationen.

Im ersten Schritt müssen Sie sicherstellen, dass das Projekt conjesouniversity als Startprojekt festgelegt ist. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt conjesouniversity, und wählen Sie **als Startprojekt festlegen**aus. Code First-Migrationen sucht im Startprojekt nach der Daten bankverbindungs Zeichenfolge.

Klicken Sie im Menü **Extras** auf **NuGet-Paket-Manager** und dann auf **Paket-Manager-Konsole**.

![Selecting_Package_Manager_Console](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image6.png)

Wählen Sie oben im Konsolenfenster des **Paket-Managers** die Option condesouniversity. dal als Standard Projekt aus, und geben Sie dann an der `PM>` Eingabeaufforderung "Enable-Migrationen" ein.

![Enable-migrations_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image7.png)

Mit diesem Befehl wird eine *Configuration.cs* -Datei in einem neuen *Migrations* Ordner im Projekt condesouniversity. dal erstellt.

![Migrations_folder_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image8.png)

Sie haben das DAL-Projekt ausgewählt, da der Befehl "Enable-Migrationen" in dem Projekt ausgeführt werden muss, das die Code First Kontext Klasse enthält. Wenn sich diese Klasse in einem Klassen Bibliotheksprojekt befindet, sucht Code First-Migrationen im Startprojekt der Projekt Mappe nach der Datenbank-Verbindungs Zeichenfolge. In der Lösung conjesouniversity wurde das Webprojekt als Startprojekt festgelegt. (Wenn Sie das Projekt, das die Verbindungs Zeichenfolge enthält, nicht als Startprojekt in Visual Studio festlegen möchten, können Sie das Startprojekt im PowerShell-Befehl angeben. Um die Befehlssyntax für den Befehl "Enable-Migrationen" anzuzeigen, können Sie den Befehl "Get-Help enable-Migrationen" eingeben.)

Öffnen Sie die Datei Configuration.cs, und ersetzen Sie die Kommentare in der `Seed`-Methode durch den folgenden Code:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample3.cs)]

Die Verweise auf `List` verfügen über rote Wellenlinien, da Sie noch keine `using`-Anweisung für Ihren Namespace haben. Klicken Sie mit der rechten Maustaste auf eine der Instanzen `List`, klicken Sie auf **Auflösen**, und klicken Sie dann auf **System. Collections. Generic**.

![Mit using-Anweisung auflösen](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image9.png)

Diese Menü Auswahl fügt dem `using`-Anweisungen am Anfang der Datei den folgenden Code hinzu.

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample4.cs)]

> [!NOTE]
> Das Hinzufügen von Code zur `Seed`-Methode ist eine von vielen Möglichkeiten, dass Sie festgelegte Daten in die Datenbank einfügen können. Eine Alternative besteht darin, den Methoden `Up` und `Down` der einzelnen Migrations Klassen Code hinzuzufügen. Die Methoden `Up` und `Down` enthalten Code, mit dem Daten Bank Änderungen implementiert werden. Beispiele hierfür finden Sie im Tutorial bereitstellen [eines Datenbankupdates](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) .
> 
> Sie können auch Code schreiben, der SQL-Anweisungen ausführt, indem Sie die `Sql`-Methode verwenden. Wenn Sie z. b. der Abteilungs Tabelle eine Budget Spalte hinzufügen und alle abteilungsbudgets im Rahmen einer Migration auf $1.000,00 initialisieren möchten, können Sie die folgende Codezeile zur `Up`-Methode für diese Migration hinzufügen:
> 
> `Sql("UPDATE Department SET Budget = 1000");`
> 
> In diesem für dieses Tutorial gezeigten Beispiel wird die `AddOrUpdate`-Methode in der `Seed`-Methode der Code First-Migrationen `Configuration`-Klasse verwendet. Code First-Migrationen Ruft die `Seed`-Methode nach jeder Migration auf, und diese Methode aktualisiert Zeilen, die bereits eingefügt wurden, oder fügt Sie ein, wenn Sie noch nicht vorhanden sind. Die `AddOrUpdate`-Methode ist möglicherweise nicht die beste Wahl für Ihr Szenario. Weitere Informationen finden Sie im Blog von Julie Lerman [mit der EF 4,3 addorupdate-Methode](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) .

Drücken Sie STRG + UMSCHALT + B, um das Projekt zu erstellen.

Der nächste Schritt ist das Erstellen einer `DbMigration`-Klasse für die erste Migration. Wenn Sie bei dieser Migration eine neue Datenbank erstellen möchten, müssen Sie die Datenbank löschen, die bereits vorhanden ist. SQL Server Compact-Datenbanken sind in *sdf* -Dateien im Ordner *App\_Data* enthalten. Erweitern Sie in **Projektmappen-Explorer**im Projekt condesouniversity den Eintrag *App\_Daten* , um die beiden SQL Server Compact-Datenbanken anzuzeigen, die durch *sdf* -Dateien dargestellt werden.

Klicken Sie mit der rechten Maustaste auf die Datei *School. sdf* und dann auf **Löschen**.

![sdf_files_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image10.png)

Geben Sie im Fenster **Paket-Manager-Konsole** den Befehl "Add-Migration Initial" ein, um die anfängliche Migration zu erstellen, und nennen Sie Sie "Initial".

![Add-migration_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image11.png)

Code First-Migrationen erstellt eine andere Klassendatei im *Migrations* Ordner, und diese Klasse enthält Code, der das Datenbankschema erstellt.

Geben Sie in der **Paket-Manager-Konsole**den Befehl "Update-Database" ein, um die Datenbank zu erstellen, und führen Sie die **Seed** -Methode aus.

![update-database_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image12.png)

(Wenn Sie eine Fehlermeldung erhalten, die anzeigt, dass eine Tabelle bereits vorhanden ist und nicht erstellt werden kann, liegt dies wahrscheinlich daran, dass Sie die Anwendung ausgeführt haben, nachdem Sie die Datenbank gelöscht haben und `update-database`. Löschen Sie in diesem Fall die Datei " *School. sdf* " erneut, und wiederholen Sie den `update-database` Befehl.)

Führen Sie die Anwendung aus. Die Seite "Studenten" ist nun leer, aber die Dozenten Seite enthält Dozenten. Dies ist das, was Sie nach der Bereitstellung der Anwendung in der Produktion erhalten.

![Empty_Students_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image13.png)

![Instructors_page_after_initial_migration](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image14.png)

Das Projekt ist nun bereit für die Bereitstellung der Datenbank " *School* ".

## <a name="creating-a-membership-database-for-deployment"></a>Erstellen einer Mitgliedschafts Datenbank für die Bereitstellung

Die Anwendung der Anwendung "ASP.net" verwendet das-Mitgliedschaftssystem und die Formular Authentifizierung, um Benutzer zu authentifizieren und zu autorisieren. Eine Ihrer Seiten ist nur für Administratoren zugänglich. Um diese Seite anzuzeigen, führen Sie die Anwendung aus, und wählen Sie im Flyout-Menü unter **Kurse**die Option **Guthaben aktualisieren** aus. Die Anwendung zeigt die **Anmelde** Seite an, da nur Administratoren berechtigt sind, die Seite zum **Aktualisieren von Gutschriften** zu verwenden.

[![Log_in_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image15.png)

Melden Sie sich mit dem Kennwort "Pas $ w0rd" als "admin" an (Beachten Sie die Zahl 0 anstelle des Buchstabens "o" in "w0rd"). Nachdem Sie sich angemeldet haben, wird die Seite zum **Aktualisieren des Guthabens** angezeigt.

[![Update_Credits_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image17.png)

Wenn Sie einen Standort zum ersten Mal bereitstellen, ist es üblich, die meisten oder alle Benutzerkonten auszuschließen, die Sie zu Testzwecken erstellen. In diesem Fall stellen Sie ein Administrator Konto und keine Benutzerkonten bereit. Anstatt Testkonten manuell zu löschen, erstellen Sie eine neue Mitgliedschafts Datenbank, die nur über das Benutzerkonto eines Administrators verfügt, das Sie in der Produktionsumgebung benötigen.

> [!NOTE]
> Die Mitgliedschafts Datenbank speichert einen Hashwert für Konto Kennwörter. Um Konten von einem Computer auf einen anderen Computer bereitzustellen, müssen Sie sicherstellen, dass von den Hash Routinen keine anderen Hashes auf dem Zielserver generiert werden als auf dem Quellcomputer. Sie generieren die gleichen Hashes, wenn Sie die ASP.net-universelle Anbieter verwenden, sofern Sie den Standard Algorithmus nicht ändern. Der Standard Algorithmus ist HMACSHA256 und wird im **Validierungs** Attribut des **[machineKey](https://msdn.microsoft.com/library/w8h3skw9.aspx)** -Elements in der Datei Web. config angegeben.

Die Mitgliedschafts Datenbank wird nicht von Code First-Migrationen verwaltet, und es gibt keinen automatischen Initialisierer, der die Datenbank mit Testkonten (wie für die Datenbank "School") verwaltet. Um Testdaten verfügbar zu machen, erstellen Sie daher eine Kopie der Testdatenbank, bevor Sie eine neue erstellen.

Benennen Sie in **Projektmappen-Explorer**die Datei *ASPNET. sdf* im Ordner *App\_Data* in *ASPNET-dev. sdf*um. (Erstellen Sie keine Kopie, benennen Sie Sie um – Sie erstellen in Kürze eine neue Datenbank.)

Stellen Sie in **Projektmappen-Explorer**sicher, dass das Webprojekt (conjesouniversity, nicht conjesouniversity. DAL) ausgewählt ist. Wählen Sie dann im Menü **Projekt** die Option **ASP.NET-Konfiguration** aus, um das **Websiteverwaltungs-Tool**(Wat) auszuführen.

Wählen Sie die Registerkarte **Sicherheit** aus.

[![WAT_Security_tab](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image19.png)

Klicken Sie auf **Rollen erstellen oder verwalten** und **Administrator** Rolle hinzufügen.

[![WAT_Create_New_Role](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image21.png)

Navigieren Sie zurück zur Registerkarte **Sicherheit** , klicken Sie auf **Benutzer erstellen**, und fügen Sie als Administrator den Benutzer "admin" hinzu. Vergewissern Sie sich, dass Sie auf der Seite **Benutzer erstellen** auf die Schaltfläche **Benutzer erstellen** klicken, um das Kontrollkästchen **Administrator** auszuwählen. Das in diesem Tutorial verwendete Kennwort lautet "Pas $ w0rd", und Sie können eine beliebige e-Mail-Adresse eingeben.

[![WAT_Create_User](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image23.png)

Schließen Sie den Browser. Klicken Sie in **Projektmappen-Explorer**auf die Schaltfläche Aktualisieren, um die neue Datei *ASPNET. sdf* anzuzeigen.

![New_aspnet.sdf_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image25.png)

Klicken Sie mit der rechten Maustaste auf **ASPNET. sdf** , und wählen Sie **in Projekt einschließen**.

## <a name="distinguishing-development-from-production-databases"></a>Unterscheiden der Entwicklung von Produktionsdatenbanken

In diesem Abschnitt benennen Sie die Datenbanken so um, dass die Entwicklungsversionen School-dev. sdf und ASPNET-dev. sdf lauten, und die Produktionsversionen lauten School-Prod. sdf und ASPNET-Prod. sdf. Dies ist nicht erforderlich, aber dadurch wird verhindert, dass die Test-und Produktionsversionen der Datenbanken verwirrt werden.

Klicken Sie in **Projektmappen-Explorer**auf **Aktualisieren** , und erweitern Sie den Ordner App\_Data, um die Datenbank "School" anzuzeigen, die Sie zuvor erstellt haben. Klicken Sie mit der rechten Maustaste darauf, und wählen Sie **in Projekt einschließen**.

![Including_School. sdf_in_project](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image26.png)

Benennen Sie *ASPNET. sdf* in *ASPNET-Prod. sdf*um.

Benennen Sie *School. sdf* in *School-dev. sdf*um.

Wenn Sie die Anwendung in Visual Studio ausführen, möchten Sie nicht die *-Prod-* Versionen der Datenbankdateien verwenden. Sie möchten die *-dev-* Versionen verwenden. Daher müssen Sie die Verbindungs Zeichenfolgen in der Datei "Web. config" so ändern, dass Sie auf die *-dev-* Versionen der Datenbanken verweisen. (Sie haben noch keine School-Prod. SDF-Datei erstellt, aber das ist in Ordnung, da Code First diese Datenbank in der Produktionsumgebung erstellen, wenn Sie Ihre APP zum ersten Mal ausführen.)

Öffnen Sie die Datei Web. config der Anwendung, und suchen Sie die Verbindungs Zeichenfolgen:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample5.xml)]

Ändern Sie "ASPNET. sdf" in "ASPNET-dev. sdf", und ändern Sie "School. sdf" in "School-dev. sdf":

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample6.xml?highlight=4-5)]

Die SQL Server Compact Datenbank-Engine und beide Datenbanken können jetzt bereitgestellt werden. Im folgenden Tutorial richten Sie automatische *Web. config* -Datei Transformationen für Einstellungen ein, die sich in den Entwicklungs-, Test-und Produktionsumgebungen unterscheiden müssen. (Bei den Einstellungen, die geändert werden müssen, handelt es sich um die Verbindungs Zeichenfolgen. Sie richten diese Änderungen jedoch später ein, wenn Sie ein Veröffentlichungs Profil erstellen.)

## <a name="more-information"></a>Weitere Informationen

Weitere Informationen zu nuget finden Sie unter [Verwalten von Projekt Bibliotheken mit nuget](https://msdn.microsoft.com/magazine/hh547106.aspx) und [nuget-Dokumentation](http://docs.nuget.org/docs/start-here/overview). Wenn Sie nuget nicht verwenden möchten, müssen Sie erfahren, wie Sie ein nuget-Paket analysieren, um zu bestimmen, was es bei der Installation durchführt. (Beispielsweise können Sie *Web. config* -Transformationen konfigurieren, PowerShell-Skripts für die Lauf Zeit Erstellung konfigurieren usw.) Weitere Informationen zur Funktionsweise von nuget finden Sie unter vor allem das [Erstellen und Veröffentlichen von Paketen](http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-package) und [Konfigurationsdateien und Quell Code Transformationen](http://docs.nuget.org/docs/creating-packages/configuration-file-and-source-code-transformations).

> [!div class="step-by-step"]
> [Zurück](deployment-to-a-hosting-provider-introduction-1-of-12.md)
> [Weiter](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12.md)
