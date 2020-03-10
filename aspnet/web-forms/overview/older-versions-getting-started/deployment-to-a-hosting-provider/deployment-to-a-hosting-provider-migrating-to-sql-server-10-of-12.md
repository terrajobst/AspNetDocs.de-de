---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12
title: 'Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mithilfe von Visual Studio oder Visual Web Developer: Migrieren zu SQL Server-10 von 12 | Microsoft-Dokumentation'
author: tdykstra
description: In dieser Reihe von Tutorials wird gezeigt, wie Sie ein ASP.NET-Webanwendungs Projekt bereitstellen (veröffentlichen), das eine SQL Server Compact-Datenbank mit Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: a89d6f32-b71b-4036-8ff7-5f8ac2a6eca8
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12
msc.type: authoredcontent
ms.openlocfilehash: c5281a42596d95e725b32e652c75785abe0fd64e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78462837"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-migrating-to-sql-server---10-of-12"></a>Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mithilfe von Visual Studio oder Visual Web Developer: Migrieren zu SQL Server-10 von 12

von [Tom Dykstra](https://github.com/tdykstra)

[Starter Projekt herunterladen](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> In dieser Reihe von Tutorials erfahren Sie, wie Sie ein ASP.NET-Webanwendungs Projekt, das eine SQL Server Compact Datenbank enthält, mithilfe von Visual Studio 2012 RC oder Visual Studio Express 2012 RC für das Web bereitstellen (veröffentlichen). Sie können auch Visual Studio 2010 verwenden, wenn Sie das Webveröffentlichungs Update installieren. Eine Einführung in die Reihe finden Sie [im ersten Tutorial der Reihe](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Ein Tutorial, das nach der RC-Version von Visual Studio 2012 eingeführte Bereitstellungs Funktionen zeigt, zeigt, wie SQL Server Editionen außer SQL Server Compact bereitgestellt werden, und zeigt, wie Sie die Bereitstellung für Azure App Service Web-Apps ausführen. Weitere Informationen finden Sie unter [ASP.net Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Übersicht

In diesem Tutorial wird gezeigt, wie Sie von SQL Server Compact zu SQL Server migrieren. Ein Grund hierfür ist, dass Sie die Vorteile von SQL Server Features nutzen können, die SQL Server Compact nicht unterstützt, wie z. b. gespeicherte Prozeduren, Trigger, Sichten oder Replikation. Weitere Informationen zu den Unterschieden zwischen SQL Server Compact und SQL Server finden Sie im Tutorial bereitstellen [SQL Server Compact](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) .

### <a name="sql-server-express-versus-full-sql-server-for-development"></a>SQL Server Express im Vergleich zu vollständigen SQL Server für die Entwicklung

Wenn Sie sich für ein Upgrade auf SQL Server entschieden haben, möchten Sie möglicherweise SQL Server oder SQL Server Express in ihren Entwicklungs-und Testumgebungen verwenden. Zusätzlich zu den Unterschieden in der Tool Unterstützung und in Funktionen der Datenbank-Engine gibt es Unterschiede in den Anbieter Implementierungen zwischen SQL Server Compact und anderen Versionen von SQL Server. Diese Unterschiede können dazu führen, dass derselbe Code andere Ergebnisse generiert. Wenn Sie sich entscheiden, SQL Server Compact als Entwicklungs Datenbank beizubehalten, sollten Sie daher Ihre Website in SQL Server oder SQL Server Express in einer Testumgebung gründlich testen, bevor Sie die Bereitstellung in der Produktion durchführen.

Im Gegensatz zu SQL Server Compact ist SQL Server Express im Wesentlichen die gleiche Datenbank-Engine und verwendet denselben .NET-Anbieter wie die voll SQL Server. Wenn Sie mit SQL Server Express testen, können Sie sicher sein, dass Sie die gleichen Ergebnisse wie bei SQL Server erhalten. Sie können die meisten der gleichen Datenbanktools mit SQL Server Express verwenden, die Sie mit SQL Server verwenden können (eine wichtige Ausnahme ist [SQL Server Profiler](https://msdn.microsoft.com/library/ms181091.aspx)) und andere Features von SQL Server wie gespeicherte Prozeduren, Sichten, Trigger und Replikation unterstützen. (In der Regel müssen Sie jedoch vollständige SQL Server auf einer Produktions Website verwenden. SQL Server Express können in einer freigegebenen Hostingumgebung ausgeführt werden, aber Sie wurde nicht dafür entworfen, und viele Hostinganbieter unterstützen Sie nicht.)

Wenn Sie Visual Studio 2012 verwenden, wählen Sie in der Regel SQL Server Express localdb für Ihre Entwicklungsumgebung aus, da dies mit Visual Studio standardmäßig installiert wird. Allerdings funktioniert localdb in IIS nicht, sodass Sie für Ihre Testumgebung entweder SQL Server oder SQL Server Express verwenden müssen.

### <a name="combining-databases-versus-keeping-them-separate"></a>Kombinieren und trennen von Datenbanken

Die Anwendung der Anwendung "" der Anwendung "der Anwendung" hat zwei SQL Server Compact Datenbanken: die Mitgliedschafts Datenbank (*ASPNET. sdf*) und die Anwendungsdatenbank (*School. sdf*). Bei der Migration können Sie diese Datenbanken zu zwei separaten Datenbanken oder zu einer einzelnen Datenbank migrieren. Möglicherweise möchten Sie diese kombinieren, um datenbankjoins zwischen Ihrer Anwendungsdatenbank und der Mitgliedschafts Datenbank zu vereinfachen. Ihr Hostingplan kann auch einen Grund für die Kombination der Werte darstellen. Beispielsweise kann der Hostinganbieter mehr als eine Datenbank mehr als eine Datenbank berechnen. Dies ist der Fall mit dem in diesem Tutorial verwendeten cytanium Lite-Hostingkonto, das nur eine einzige SQL Server Datenbank zulässt.

In diesem Tutorial migrieren Sie Ihre beiden Datenbanken auf folgende Weise:

- Migrieren Sie zu zwei localdb-Datenbanken in der Entwicklungsumgebung.
- Migrieren Sie zu zwei SQL Server Express Datenbanken in der Testumgebung.
- Migrieren Sie zu einer kombinierten vollständigen SQL Server-Datenbank in der Produktionsumgebung.

Erinnerung: Wenn Sie eine Fehlermeldung erhalten oder etwas nicht funktioniert, wenn Sie das Tutorial durchlaufen, achten Sie darauf, dass Sie die [Seite Problem](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)Behandlung überprüfen.

## <a name="installing-sql-server-express"></a>Installieren von SQL Server Express

SQL Server Express wird standardmäßig automatisch mit Visual Studio 2010 installiert, wird jedoch standardmäßig nicht mit Visual Studio 2012 installiert. Um SQL Server 2012 Express zu installieren, klicken Sie auf den folgenden Link:

- [SQL Server Express 2012](https://www.microsoft.com/download/details.aspx?id=29062)

Wählen Sie deu */x64/sqlexpr\_x64\_deu. exe* oder deu */x86/sqlexpr\_x86\_deu. exe*aus, und übernehmen Sie im Installations-Assistenten die Standardeinstellungen. Weitere Informationen zu Installationsoptionen finden Sie unter [Install SQL Server 2012 from the Installation Wizard (Setup)](https://msdn.microsoft.com/library/ms143219.aspx).

## <a name="creating-sql-server-express-databases-for-the-test-environment"></a>Erstellen von SQL Server Express-Datenbanken für die Test Umgebung

Der nächste Schritt besteht im Erstellen der ASP.net-Mitgliedschafts-und Schul Datenbanken.

Wählen Sie im Menü **Ansicht** die Option **Server-Explorer** (**Datenbank-Explorer** in Visual Web Developer) aus, klicken Sie dann mit der rechten Maustaste auf **Datenverbindungen** , und wählen Sie **neue SQL Server Datenbank erstellen**aus.

![Selecting_Create_New_SQL_Server_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image1.png)

Geben Sie im Dialogfeld **neue SQL Server Datenbank erstellen** im Feld **Server Name** den Namen ".\sqlexpress" ein, und klicken Sie im Feld Name der **neuen Datenbank** auf "ASPNET-Test". Klicken Sie dann auf **OK**.

![Create_New_SQL_Server_Database_aspnet](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image2.png)

Führen Sie das gleiche Verfahren aus, um eine neue SQL Server Express School-Datenbank mit dem Namen "School-Test" zu erstellen.

(Sie fügen "Test" an diese Datenbanknamen an, da Sie später eine zusätzliche Instanz der einzelnen Datenbanken für die Entwicklungsumgebung erstellen und Sie in der Lage sein müssen, die beiden Gruppen von Datenbanken zu unterscheiden.)

**Server-Explorer** zeigt nun die beiden neuen Datenbanken an.

![New_databases_in_Server_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image3.png)

## <a name="creating-a-grant-script-for-the-new-databases"></a>Erstellen eines Grant-Skripts für die neuen Datenbanken

Wenn die Anwendung in IIS auf dem Entwicklungs Computer ausgeführt wird, greift die Anwendung mithilfe der Anmelde Informationen des Standard Anwendungs Pools auf die Datenbank zu. Standardmäßig verfügt die Anwendungs Pool Identität jedoch nicht über die Berechtigung zum Öffnen der Datenbanken. Sie müssen also ein Skript ausführen, um diese Berechtigung zu erteilen. In diesem Abschnitt erstellen Sie das Skript, das Sie später ausführen werden, um sicherzustellen, dass die Anwendung die Datenbanken öffnen kann, wenn Sie in IIS ausgeführt wird.

Erstellen Sie im *Projektmappenordner* Projektmappendatei, den Sie im Tutorial bereitstellen [in der Produktionsumgebung](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) erstellt haben, eine neue SQL-Datei mit dem Namen *Grant. SQL*. Kopieren Sie die folgenden SQL-Befehle in die Datei, und speichern und schließen Sie dann die Datei:

[!code-sql[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample1.sql)]

> [!NOTE]
> Dieses Skript ist so konzipiert, dass es mit SQL Server 2008 und mit den IIS-Einstellungen in Windows 7 funktioniert, wie Sie in diesem Tutorial angegeben sind. Wenn Sie eine andere Version von SQL Server oder Windows verwenden oder IIS auf dem Computer anders einrichten, sind möglicherweise Änderungen an diesem Skript erforderlich. Weitere Informationen zu SQL Server Skripts finden Sie unter [SQL Server-Onlinedokumentation](https://go.microsoft.com/fwlink/?LinkId=132511).

> [!NOTE] 
> 
> **Sicherheitshinweis** Mit diesem Skript erhalten Daten Bank\_Besitzer Berechtigungen für den Benutzer, der zur Laufzeit auf die Datenbank zugreift, was Sie in der Produktionsumgebung tun werden. In einigen Szenarien möchten Sie möglicherweise einen Benutzer angeben, der über vollständige Datenbankschema-Aktualisierungs Berechtigungen nur für die Bereitstellung verfügt, und für die Laufzeit einen anderen Benutzer angeben, der nur über Berechtigungen zum Lesen und Schreiben von Daten verfügt. Weitere Informationen finden Sie unter über **Prüfen der automatischen Web. config-Änderungen für Code First-Migrationen bei der** Bereitstellung in [IIS als Test Umgebung](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md).

## <a name="configuring-database-deployment-for-the-test-environment"></a>Konfigurieren der Daten Bank Bereitstellung für die Test Umgebung

Als Nächstes konfigurieren Sie Visual Studio so, dass für jede Datenbank die folgenden Aufgaben ausgeführt werden:

- Generieren Sie ein SQL-Skript, das die Struktur der Quelldatenbank (Tabellen, Spalten, Einschränkungen usw.) in der Zieldatenbank erstellt.
- Generieren Sie ein SQL-Skript, das die Daten der Quelldatenbank in die Tabellen in der Zieldatenbank einfügt.
- Führen Sie die generierten Skripts und das von Ihnen erstellte Grant-Skript in der Zieldatenbank aus.

Öffnen Sie das Fenster **Projekteigenschaften** , und wählen Sie die Registerkarte **SQL Verpacken/veröffentlichen** aus.

Stellen Sie sicher, dass in der Dropdown Liste **Konfiguration** die Option **aktiv (Release)** oder **Release** ausgewählt ist.

Klicken Sie auf **Diese Seite aktivieren**.

![Package_Publish_SQL_tab_Enable_This_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image4.png)

Die Registerkarte **Paket-/Veröffentlichungs-SQL** ist normalerweise deaktiviert, da Sie eine Legacy Bereitstellungs Methode angibt. In den meisten Szenarien sollten Sie die Daten Bank Bereitstellung im Assistenten **Web veröffentlichen** konfigurieren. Das Migrieren von SQL Server Compact zu SQL Server oder SQL Server Express ist ein Sonderfall, bei dem diese Methode eine gute Wahl ist.

Klicken Sie auf **aus "Web. config" Importieren**.

![Selecting_Import_from_Web.config](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image5.png)

Visual Studio sucht in der Datei " *Web. config* " nach Verbindungs Zeichenfolgen, sucht einen für die Mitgliedschafts Datenbank und einen für die Datenbank "School" und fügt eine Zeile hinzu, die jeder Verbindungs Zeichenfolge in der Tabelle " **Datenbankeinträge** " entspricht. Die gefundenen Verbindungs Zeichenfolgen gelten für die vorhandenen SQL Server Compact-Datenbanken. der nächste Schritt besteht darin, zu konfigurieren, wie und wo diese Datenbanken bereitgestellt werden sollen.

Sie geben Einstellungen für die Daten Bank Bereitstellung im Abschnitt **Daten Bank Eingabe Details** unterhalb der Tabelle **Datenbankeinträge** ein. Die im Abschnitt Details zum **Datenbankeintrag** angezeigten Einstellungen beziehen sich auf die ausgewählte Zeile in der Tabelle **Datenbankeinträge** , wie in der folgenden Abbildung dargestellt.

![Database_Entry_Details_section_of_Package_Publish_SQL_tab](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image6.png)

### <a name="configuring-deployment-settings-for-the-membership-database"></a>Konfigurieren von Bereitstellungs Einstellungen für die Mitgliedschafts Datenbank

Wählen Sie die Zeile **DefaultConnection-Deployment** in der Tabelle **Datenbankeinträge** aus, um Einstellungen zu konfigurieren, die für die Mitgliedschafts Datenbank gelten.

Geben Sie unter **Verbindungs Zeichenfolge für Zieldatenbank**eine Verbindungs Zeichenfolge ein, die auf die neue SQL Server Express Mitgliedschafts Datenbank verweist. Sie können die benötigte Verbindungs Zeichenfolge von **Server-Explorer**erhalten. ErweiternSie in Server-Explorer **Datenverbindungen** , und wählen Sie die **aspnetTest** -Datenbank aus, und kopieren Sie dann im **Eigenschaften** Fenster den Wert der **Verbindungs Zeichenfolge** .

![aspnet_connection_string_in_Server_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image7.png)

Dieselbe Verbindungs Zeichenfolge wird hier reproduziert:

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample2.cmd)]

Kopieren Sie diese Verbindungs Zeichenfolge, und fügen Sie Sie in die **Verbindungs Zeichenfolge für die Zieldatenbank** auf der Registerkarte " **Verpacken**

Stellen Sie sicher, dass **Pull Data und/oder Schema aus einer vorhandenen Datenbank** ausgewählt ist. Dies bewirkt, dass SQL-Skripts automatisch generiert und in der Zieldatenbank ausgeführt werden.

Die **Verbindungs Zeichenfolge für den Wert der Quelldatenbank** wird aus der Datei " *Web. config* " extrahiert und verweist auf die Entwicklungs SQL Server Compact Datenbank. Dies ist die Quelldatenbank, die verwendet wird, um die Skripts zu generieren, die später in der Zieldatenbank ausgeführt werden. Da Sie die Produktionsversion der Datenbank bereitstellen möchten, ändern Sie "ASPNET-dev. sdf" in "ASPNET-Prod. sdf".

Ändern Sie die Daten **Bank Skript Optionen** aus dem **Schema nur** in das **Schema und die Daten**, da Sie die Daten (Benutzerkonten und Rollen) sowie die Datenbankstruktur kopieren möchten.

Um die Bereitstellung so zu konfigurieren, dass die zuvor erstellten Grant-Skripts ausgeführt werden, müssen Sie Sie dem Abschnitt **Daten Bank Skripts** hinzufügen. Klicken Sie auf **Skript hinzufügen**, und navigieren Sie im Dialogfeld **SQL-Skripts hinzufügen** zu dem Ordner, in dem Sie das Grant-Skript gespeichert haben (der Ordner, der die Projektmappendatei enthält). Wählen Sie die Datei *Grant. SQL*aus, und klicken Sie auf **Öffnen**.

[![Select_File_dialog_box_grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image9.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image8.png)

Die Einstellungen für die Zeile **DefaultConnection-Deployment** in den **Datenbankeinträgen** sehen nun wie in der folgenden Abbildung aus:

![Database_Entry_Details_for_DefaultConnection_Test](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image10.png)

### <a name="configuring-deployment-settings-for-the-school-database"></a>Konfigurieren von Bereitstellungs Einstellungen für die Datenbank "School"

Wählen Sie als nächstes die Zeile **schoolContext-Deployment** in der Tabelle **Datenbankeinträge** aus, um die Bereitstellungs Einstellungen für die Datenbank "School" zu konfigurieren.

Sie können dieselbe Methode verwenden, die Sie zuvor verwendet haben, um die Verbindungs Zeichenfolge für die neue SQL Server Express-Datenbank zu erhalten. Kopieren Sie diese Verbindungs Zeichenfolge in die **Verbindungs Zeichenfolge für die Zieldatenbank** auf der Registerkarte **packen/veröffentlichen** .

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample3.cmd)]

Stellen Sie sicher, dass **Pull Data und/oder Schema aus einer vorhandenen Datenbank** ausgewählt ist.

Die **Verbindungs Zeichenfolge für den Wert der Quelldatenbank** wird aus der Datei " *Web. config* " extrahiert und verweist auf die Entwicklungs SQL Server Compact Datenbank. Ändern Sie "School-dev. sdf" in "School-Prod. sdf", um die Produktionsversion der Datenbank bereitzustellen. (Sie haben nie eine School-Prod. SDF-Datei im Ordner "App\_Data" erstellt. daher kopieren Sie diese Datei aus der Testumgebung in den Ordner "App\_Data" im Projektordner "condesouniversity".)

Ändern Sie die **Datenbankskript Optionen** in **Schema und Daten**.

Außerdem möchten Sie das Skript ausführen, um der Anwendungs Pool Identität Lese-und Schreibberechtigungen für diese Datenbank zu erteilen. Fügen Sie daher die Skriptdatei " *Grant. SQL* " wie für die Mitgliedschafts Datenbank hinzu.

Wenn Sie fertig sind, sehen die Einstellungen für die Zeile **schoolContext-Deployment** in den **Datenbankeinträgen** wie in der folgenden Abbildung aus:

![Database_Entry_Details_for_SchoolContext_Test](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image11.png)

Speichern Sie die Änderungen auf der Registerkarte " **packen/veröffentlichen** ".

Kopieren Sie die Datei *School-Prod. sdf* aus dem Ordner *c:\inetpub\wwwroot\condesouniversity\app\_Data* in den Ordner *App\_Data* im Projekt condesouniversity.

### <a name="specifying-transacted-mode-for-the-grant-script"></a>Angeben des transaktiven Modus für das Grant-Skript

Der Bereitstellungs Prozess generiert Skripts, die das Datenbankschema und die Daten bereitstellen. Standardmäßig werden diese Skripts in einer Transaktion ausgeführt. Benutzerdefinierte Skripts (z. b. die Grant-Skripts) werden jedoch standardmäßig nicht in einer Transaktion ausgeführt. Wenn der Bereitstellungs Prozess den Transaktionsmodus vermischt, tritt möglicherweise ein Timeout Fehler auf, wenn die Skripts während der Bereitstellung ausgeführt werden. In diesem Abschnitt Bearbeiten Sie die Projektdatei, um die benutzerdefinierten Skripts so zu konfigurieren, dass Sie in einer Transaktion ausgeführt werden.

Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt **conjesouniversity** , und wählen Sie **Projekt entladen**aus.

![Unload_Project_in_Solution_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image12.png)

Klicken Sie dann mit der rechten Maustaste erneut auf das Projekt, und wählen Sie **condesouniversity. csproj bearbeiten**aus.

![Edit_Project_in_Solution_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image13.png)

Der Visual Studio-Editor zeigt den XML-Inhalt der Projektdatei an. Beachten Sie, dass mehrere `PropertyGroup` Elemente vorhanden sind. (In der Abbildung wurden die Inhalte der `PropertyGroup` Elemente weggelassen.)

![Fenster "Projektdatei-Editor"](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image15.png)

Der erste, der über kein `Condition` Attribut verfügt, ist für Einstellungen vorgesehen, die unabhängig von der Buildkonfiguration angewendet werden. Ein `PropertyGroup`-Element gilt nur für die Debug-Buildkonfiguration (Beachten Sie das `Condition`-Attribut), ein Element gilt nur für die releasebuildkonfiguration, und ein Element gilt nur für die testbuildkonfiguration. Innerhalb des `PropertyGroup`-Elements für die releasebuildkonfiguration wird ein `PublishDatabaseSettings` Element angezeigt, das die Einstellungen enthält, die Sie auf der Registerkarte " **packen/veröffentlichen** " eingegeben haben. Es gibt ein `Object` Element, das jedem der von Ihnen angegebenen Grant-Skripts entspricht (Beachten Sie die beiden Instanzen von "Grant. SQL"). Standardmäßig ist das `Transacted`-Attribut des `Source`-Elements für jedes Grant-Skript `False`.

![Transacted_false](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image16.png)

Ändern Sie den Wert des `Transacted`-Attributs des Elements `Source` in `True`.

![Transacted_true](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image17.png)

Speichern und schließen Sie die Projektdatei, und klicken Sie dann in **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie **Projekt erneut laden**.

![Reload_project](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image18.png)

## <a name="setting-up-webconfig-transformations-for-the-connection-strings"></a>Einrichten von Web. config-Transformationen für die Verbindungs Zeichenfolgen

Die Verbindungs Zeichenfolgen für die neuen SQL Express-Datenbanken, die Sie auf der Registerkarte **Verpacken/veröffentlichen-SQL** eingegeben haben, werden von Web deploy nur zum Aktualisieren der Zieldatenbank während der Bereitstellung verwendet. Sie müssen weiterhin *Web. config* -Transformationen einrichten, damit die Verbindungs Zeichenfolgen in der bereitgestellten *Web. config* -Datei auf die neuen SQL Server Express-Datenbanken verweisen. (Wenn Sie die Registerkarte " **packen/veröffentlichen** " verwenden, können Sie keine Verbindungs Zeichenfolgen im Veröffentlichungs Profil konfigurieren.)

Öffnen Sie *Web. Test. config* , und ersetzen Sie das `connectionStrings`-Element durch das `connectionStrings`-Element im folgenden Beispiel. (Stellen Sie sicher, dass Sie nur das connectionStrings-Element kopieren, nicht den umgebenden Code, der hier angezeigt wird, um Kontext bereitzustellen.)

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample4.xml?highlight=2-11)]

Dieser Code bewirkt, dass die `connectionString`-und `providerName` Attribute der einzelnen `add` Elemente in der bereitgestellten *Web. config* -Datei ersetzt werden. Diese Verbindungs Zeichenfolgen sind nicht identisch mit denen, die Sie auf der Registerkarte " **packen/veröffentlichen** " eingegeben haben. Die Einstellung "MultipleActiveResultSets = True" wurde hinzugefügt, da Sie für die Entity Framework und die universelle Anbieter erforderlich ist.

## <a name="installing-sql-server-compact"></a>Installieren von SQL Server Compact

Das nuget-Paket sqlservercompact stellt die SQL Server Compact Datenbank-Engine-Assemblys für die Anwendung "der Anwendung" der Anwendung ". Jetzt ist es aber nicht die Anwendung, sondern Web deploy, die in der Lage sein müssen, die SQL Server Compact-Datenbanken zu lesen, um Skripts zum Ausführen in den SQL Server-Datenbanken zu erstellen. Um Web deploy SQL Server Compact-Datenbanken lesen zu können, installieren Sie SQL Server Compact auf dem Entwicklungs Computer, indem Sie den folgenden Link verwenden: [Microsoft SQL Server Compact 4,0](https://www.microsoft.com/downloads/details.aspx?FamilyID=15F7C9B3-A150-4AD2-823E-E4E0DCF85DF6).

## <a name="deploying-to-the-test-environment"></a>Bereitstellen in der Test Umgebung

Zum Veröffentlichen in der Test Umgebung müssen Sie ein Veröffentlichungs Profil erstellen, das so konfiguriert ist, dass die Registerkarte " **Paket/veröffentlichen** " für die Daten Bank Veröffentlichung anstelle der Veröffentlichungs Profil-Datenbankeinstellungen verwendet wird.

Löschen Sie zunächst das vorhandene Test Profil.

Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt conjesouniversity, und klicken Sie dann auf **veröffentlichen**.

Wählen Sie die Registerkarte **Profil** aus.

Klicken Sie auf **Manage Profiles** (Profile verwalten).

Wählen Sie **Test**, klicken Sie auf **Entfernen**, und klicken Sie dann auf **Schließen**.

Schließen Sie den Assistenten **Web veröffentlichen** , um diese Änderung zu speichern.

Erstellen Sie als nächstes ein neues Test Profil, und verwenden Sie es zum Veröffentlichen des Projekts.

Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt conjesouniversity, und klicken Sie dann auf **veröffentlichen**.

Wählen Sie die Registerkarte **Profil** aus.

Wählen Sie in der Dropdown Liste **&lt;neu...&gt;** aus, und geben Sie "Test" als Profilnamen ein.

Geben Sie im Feld **Dienst-URL** den *Namen localhost*ein.

Geben Sie im Feld **Website/Anwendung** den Eintrag *Default Web Site/Conto souniversity*ein.

Geben Sie im Feld **Ziel-URL** `http://localhost/ContosoUniversity/`ein.

Klicken Sie auf **Weiter**.

Auf der Registerkarte " **Einstellungen** " wird gewarnt, dass die Registerkarte " **SQL packen/veröffentlichen** " konfiguriert wurde, und Sie haben die Möglichkeit, Sie zu überschreiben, indem Sie auf neue Verbesserungen der Daten Bank Veröffentlichung Für diese Bereitstellung möchten Sie die Einstellungen für das **packen/Veröffentlichen von SQL-** Registerkarten nicht außer Kraft setzen. Klicken Sie daher einfach auf **weiter**

![Publish_Web_wizard_Settings_tab_Migrate](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image19.png)

Eine Meldung auf der Registerkarte **Vorschau** gibt an, dass **keine Datenbanken für die Veröffentlichung ausgewählt sind**. Dies bedeutet jedoch nur, dass die Daten Bank Veröffentlichung nicht im Veröffentlichungs Profil konfiguriert ist.

Klicken Sie auf **Veröffentlichen**.

![Publish_Web_wizard_Preview_tab_Migrate](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image20.png)

Visual Studio stellt die Anwendung bereit und öffnet den Browser auf der Startseite der Website in der Testumgebung. Führen Sie die Seite Dozenten aus, um zu sehen, dass die gleichen Daten angezeigt werden, die Sie zuvor gesehen haben. Führen Sie die Seite **Studenten hinzufügen** aus, fügen Sie einen neuen Studenten hinzu, und zeigen Sie dann den neuen Studenten auf der Seite **Studenten** an. Dadurch wird überprüft, ob Sie die Datenbank aktualisieren können. Wählen Sie die Seite **Update Guthaben** (Sie müssen sich anmelden) aus, um zu überprüfen, ob die Mitgliedschafts Datenbank bereitgestellt wurde und Sie darauf zugreifen können.

## <a name="creating-a-sql-server-database-for-the-production-environment"></a>Erstellen einer SQL Server-Datenbank für die Produktionsumgebung

Nachdem Sie die Bereitstellung in der Testumgebung abgeschlossen haben, können Sie die Bereitstellung für die Produktion einrichten. Sie beginnen wie bei der Testumgebung, indem Sie eine Datenbank für die Bereitstellung erstellen. Wie Sie sich aus der Übersicht erinnern, lässt der cytanium Lite-Hostingplan nur eine einzige SQL Server Datenbank zu, sodass Sie nur eine Datenbank einrichten, nicht zwei. Alle Tabellen und Daten aus den Datenbanken der Mitgliedschafts-und Schul SQL Server Compact werden in einer SQL Server Datenbank in der Produktionsumgebung bereitgestellt.

Wechseln Sie in der Systemsteuerung von cytanium [http://panel.cytanium.com](http://panel.cytanium.com). Halten Sie die Maus über **Datenbanken** , und klicken Sie dann auf **SQL Server 2008**.

[![Selecting_Databases_in_Control_Panel](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image22.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image21.png)

Klicken Sie auf der Seite **SQL Server 2008** auf **Datenbank erstellen**.

[![Selecting_Create_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image24.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image23.png)

Benennen Sie die Datenbank "School", und klicken Sie auf **Speichern**. (Auf der Seite wird automatisch das Präfix "" "" "" "" "".

[![Naming_the_database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image26.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image25.png)

Klicken Sie auf der gleichen Seite auf **Benutzer erstellen**. Anstatt die integrierte Windows-Sicherheit zu verwenden und die Anwendungs Pool Identität Ihre Datenbank zu öffnen, erstellen Sie auf den Servern von cytanium einen Benutzer, der über die Berechtigung zum Öffnen der Datenbank verfügt. Die Anmelde Informationen des Benutzers werden den Verbindungs Zeichenfolgen hinzugefügt, die in der *Web. config* -Produktions Datei gespeichert werden. In diesem Schritt erstellen Sie diese Anmelde Informationen.

[![Creating_a_database_user](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image28.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image27.png)

Füllen Sie die erforderlichen Felder auf der Seite **SQL-Benutzereigenschaften** aus:

- Geben Sie "Conto souniversityuser" als Namen ein.
- Geben Sie ein Kennwort ein.
- Wählen **Sie** als Standarddatenbank die Option "Configuration Manager" aus.
- **Aktivieren Sie** das Kontrollkästchen "Configuration Manager".

[![SQL_User_Properties_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image30.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image29.png)

## <a name="configuring-database-deployment-for-the-production-environment"></a>Konfigurieren der Daten Bank Bereitstellung für die Produktionsumgebung

Nun können Sie die Einstellungen für die Daten Bank Bereitstellung auf der Registerkarte " **packen/veröffentlichen** " auf der Registerkarte "SQL" wie zuvor für die Testumgebung einrichten.

Öffnen Sie das Fenster " **Projekteigenschaften** ", wählen Sie die Registerkarte " **SQL packen/veröffentlichen** " aus, und vergewissern Sie sich, dass in der Dropdown Liste **Konfiguration** die Option **aktiv (Release)** oder **Release** ausgewählt ist.

Wenn Sie die Bereitstellungs Einstellungen für die einzelnen Datenbanken konfigurieren, besteht der Hauptunterschied zwischen der Vorgehensweise bei Produktions-und Testumgebungen darin, wie Sie Verbindungs Zeichenfolgen konfigurieren. In der Testumgebung haben Sie verschiedene Verbindungs Zeichenfolgen für die Zieldatenbank eingegeben, aber für die Produktionsumgebung ist die Ziel Verbindungs Zeichenfolge für beide Datenbanken identisch. Dies liegt daran, dass Sie beide Datenbanken in einer Produktionsdatenbank bereitstellen.

### <a name="configuring-deployment-settings-for-the-membership-database"></a>Konfigurieren von Bereitstellungs Einstellungen für die Mitgliedschafts Datenbank

Um Einstellungen zu konfigurieren, die für die Mitgliedschafts Datenbank gelten, wählen Sie die Zeile **DefaultConnection-Deployment** in der Tabelle **Datenbankeinträge** aus.

Geben Sie unter **Verbindungs Zeichenfolge für Zieldatenbank**eine Verbindungs Zeichenfolge ein, die auf die neue Produktions SQL Server Datenbank verweist, die Sie soeben erstellt haben. Sie können die Verbindungs Zeichenfolge aus der Willkommens-e-Mail erhalten. Der relevante Teil der e-Mail enthält die folgende Beispiel Verbindungs Zeichenfolge:

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample5.cmd)]

Nachdem Sie die drei Variablen ersetzt haben, sieht die benötigte Verbindungs Zeichenfolge wie im folgenden Beispiel aus:

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample6.cmd)]

Kopieren Sie diese Verbindungs Zeichenfolge, und fügen Sie Sie in die **Verbindungs Zeichenfolge für die Zieldatenbank** auf der Registerkarte " **Verpacken**

Stellen Sie sicher, dass die **Pull-Daten und/oder das Schema aus einer vorhandenen Datenbank** noch ausgewählt sind, und dass die **Datenbank-Skript** Erstellungs Optionen weiterhin **Schema und Daten**sind.

Deaktivieren Sie im Feld **Daten Bank Skripts** das Kontrollkästchen neben dem Skript Grant. SQL.

![Disable_Grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image31.png)

### <a name="configuring-deployment-settings-for-the-school-database"></a>Konfigurieren von Bereitstellungs Einstellungen für die Datenbank "School"

Wählen Sie als nächstes die Zeile **schoolContext-Deployment** in der Tabelle **Datenbankeinträge** aus, um die Einstellungen für die Datenbank "School" zu konfigurieren.

Kopieren Sie dieselbe Verbindungs Zeichenfolge in die **Verbindungs Zeichenfolge für die Zieldatenbank** , die Sie in dieses Feld für die Mitglieds Datenbank kopiert haben.

Stellen Sie sicher, dass die **Pull-Daten und/oder das Schema aus einer vorhandenen Datenbank** noch ausgewählt sind, und dass die **Datenbank-Skript** Erstellungs Optionen weiterhin **Schema und Daten**sind.

Deaktivieren Sie im Feld **Daten Bank Skripts** das Kontrollkästchen neben dem Skript Grant. SQL.

Speichern Sie die Änderungen auf der Registerkarte " **packen/veröffentlichen** ".

## <a name="setting-up-webconfig-transforms-for-the-connection-strings-to-production-databases"></a>Einrichten von Web. config-Transformationen für die Verbindungs Zeichenfolgen in Produktionsdatenbanken

Als nächstes richten Sie *Web. config* -Transformationen so ein, dass die Verbindungs Zeichenfolgen in der bereitgestellten *Web. config* -Datei auf die neue Produktionsdatenbank verweisen. Die Verbindungs Zeichenfolge, die Sie für die zu verwendende Web deploy auf der Registerkarte **Package/Publish SQL** eingegeben haben, ist identisch mit der Option `MultipleResultSets`.

Öffnen Sie *Web. Production. config* , und ersetzen Sie das `connectionStrings`-Element durch ein `connectionStrings` Element, das wie im folgenden Beispiel aussieht. (Kopieren Sie nur das `connectionStrings` Element, nicht die umgebenden Tags, die bereitgestellt werden, um den Kontext anzuzeigen.)

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample7.xml?highlight=2-11)]

Manchmal werden Sie aufgefordert, Verbindungs Zeichenfolgen in der Datei " *Web. config* " immer zu verschlüsseln. Dies ist möglicherweise sinnvoll, wenn Sie die Bereitstellung auf Servern im Unternehmensnetzwerk durchsetzen. Wenn Sie jedoch in einer freigegebenen Hostingumgebung bereitstellen, Vertrauen Sie den Sicherheitsmethoden des hostinganbieters, und es ist nicht erforderlich oder praktikabel, die Verbindungs Zeichenfolgen zu verschlüsseln.

## <a name="deploying-to-the-production-environment"></a>Bereitstellen in der Produktionsumgebung

Jetzt sind Sie bereit für die Bereitstellung in der Produktion. Web deploy liest die SQL Server Compact Datenbanken in der *App\_Daten* Ordner Ihres Projekts und erstellt alle Tabellen und Daten in der Produktions SQL Server Datenbank neu. Zum Veröffentlichen mithilfe der Registerkarten Einstellungen für das **packen/Veröffentlichen von Web** müssen Sie ein neues Veröffentlichungs Profil für die Produktion erstellen.

Löschen Sie zunächst das vorhandene Produktionsprofil wie zuvor mit dem Test Profil.

Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt conjesouniversity, und klicken Sie dann auf **veröffentlichen**.

Wählen Sie die Registerkarte **Profil** aus.

Klicken Sie auf **Manage Profiles** (Profile verwalten).

Wählen Sie **Produktion**, klicken Sie auf **Entfernen**, und klicken Sie dann auf **Schließen**.

Schließen Sie den Assistenten **Web veröffentlichen** , um diese Änderung zu speichern.

Erstellen Sie als nächstes ein neues Produktionsprofil, und verwenden Sie es zum Veröffentlichen des Projekts.

Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt conjesouniversity, und klicken Sie dann auf **veröffentlichen**.

Wählen Sie die Registerkarte **Profil** aus.

Klicken Sie auf **importieren**, und wählen Sie die publishsettings-Datei aus, die Sie zuvor heruntergeladen haben.

Ändern Sie auf der Registerkarte **Verbindung** die **Ziel-URL** in die korrekte temporäre URL, die in diesem Beispiel http://contosouniversity.com.vserver01.cytanium.comist.

Benennen Sie das Profil in Production um. (Wählen Sie die Registerkarte **Profil** aus, und klicken Sie hierzu auf **Profile verwalten** .)

Schließen Sie den Assistenten **Web veröffentlichen** , um die Änderungen zu speichern.

In einer realen Anwendung, in der die Datenbank in der Produktionsumgebung aktualisiert wurde, führen Sie vor der Veröffentlichung zwei weitere Schritte aus:

1. Laden Sie *App\_offline. htm*hoch, wie im Tutorial bereitstellen [in der Produktionsumgebung](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) gezeigt.
2. Verwenden Sie **die Datei-Manager** -Funktion der Systemsteuerung cytanium, um die Dateien *ASPNET-Prod. sdf* und *School-Prod. sdf* von der Produktions Website in den Ordner *App\_Data* des condesouniversity-Projekts zu kopieren. Dadurch wird sichergestellt, dass die Daten, die Sie für die neue SQL Server-Datenbank bereitstellen, die neuesten Updates ihrer Produktions Website enthalten.

Klicken Sie im **Web auf** die Symbolleiste veröffentlichen, stellen Sie sicher, dass das **Produktions** Profil ausgewählt ist, und klicken Sie dann auf **veröffentlichen**.

Wenn Sie vor der Veröffentlichung <em>App\_offline. htm</em> hochgeladen haben, müssen Sie das <strong>Datei-Manager</strong> -Hilfsprogramm in der Systemsteuerung cytanium verwenden, um die <em>App\_Offline</em> zu löschen. htm vor dem testen. Sie können auch gleichzeitig die <em>sdf</em> -Dateien aus dem <em>App-\_Daten</em> Ordner löschen.

Sie können nun einen Browser öffnen und zu der URL Ihrer öffentlichen Website navigieren, um die Anwendung auf die gleiche Weise zu testen, die Sie nach der Bereitstellung in der Testumgebung durchgeführt haben.

## <a name="switching-to-sql-server-express-localdb-in-development"></a>Wechseln zu SQL Server Express localdb in der Entwicklung

Wie in der Übersicht erläutert, empfiehlt es sich im Allgemeinen, die gleiche Datenbank-Engine in der Entwicklung zu verwenden, die Sie in Test-und Produktionsumgebungen verwenden. (Beachten Sie, dass der Vorteil der Verwendung von SQL Server Express in der Entwicklung darin besteht, dass die Datenbank in ihren Entwicklungs-, Test-und Produktionsumgebungen identisch funktioniert.) In diesem Abschnitt richten Sie das Projekt conysouniversity so ein, dass SQL Server Express localdb verwendet wird, wenn Sie die Anwendung in Visual Studio ausführen.

Die einfachste Möglichkeit, diese Migration durchzuführen, besteht darin, Code First und das Mitgliedschaftssystem neue Entwicklungs Datenbanken für Sie zu erstellen. Für die Verwendung dieser Methode für die Migration sind drei Schritte erforderlich:

1. Ändern Sie die Verbindungs Zeichenfolgen, um neue SQL Express localdb-Datenbanken anzugeben.
2. Führen Sie das Websiteverwaltungs-Tool aus, um einen Administrator Benutzer zu erstellen. Dadurch wird die Mitgliedschafts Datenbank erstellt.
3. Verwenden Sie den Code First-Migrationen Update-Database-Befehl, um die Anwendungsdatenbank zu erstellen und zu erstellen.

### <a name="updating-connection-strings-in-the-webconfig-file"></a>Aktualisieren von Verbindungs Zeichenfolgen in der Datei "Web. config"

Öffnen Sie die Datei " *Web. config* ", und ersetzen Sie das `connectionStrings`-Element durch den folgenden Code:

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample8.xml)]

### <a name="creating-the-membership-database"></a>Erstellen der Mitgliedschafts Datenbank

Wählen Sie in **Projektmappen-Explorer**das Projekt condesouniversity aus, und klicken Sie dann im Menü **Projekt** auf **ASP.NET-Konfiguration** .

Wählen Sie die Registerkarte Sicherheit aus.

Klicken Sie auf **Rollen erstellen oder verwalten**, und erstellen Sie dann eine **Administrator** Rolle.

Kehren Sie zur Registerkarte Sicherheit zurück.

Klicken Sie auf **Benutzer erstellen**, und aktivieren Sie dann das Kontrollkästchen **Administrator** , und erstellen Sie einen Benutzer mit dem Namen admin.

Schließen Sie das **Websiteverwaltungs-Tool**.

### <a name="creating-the-school-database"></a>Erstellen der Datenbank "School"

Öffnen Sie das Fenster "Paket-Manager-Konsole".

Wählen Sie in der Dropdown Liste **Standard Projekt** das Projekt conjesouniversity. dal aus.

Geben Sie folgenden Befehl ein:

[!code-powershell[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample9.ps1)]

Code First-Migrationen wendet die anfängliche Migration an, mit der die Datenbank erstellt wird, und wendet dann die Migration addbirthdate an und führt dann die Seed-Methode aus.

Führen Sie die Website durch Drücken von STRG + F5 aus. Führen Sie wie bei den Test-und Produktionsumgebungen die Seite " **Studenten hinzufügen** " aus, fügen Sie einen neuen Studenten hinzu, und zeigen Sie dann den neuen Studenten auf der Seite " **Students** " an. Dadurch wird überprüft, ob die Datenbank "School" erstellt und initialisiert wurde und ob Sie über Lese-und Schreibzugriff verfügen.

Wählen Sie die Seite **Update Guthaben** aus, und melden Sie sich an, um zu überprüfen, ob die Mitgliedschafts Datenbank bereitgestellt wurde und Sie darauf zugreifen können. Wenn Sie Ihre Benutzerkonten nicht migriert haben, erstellen Sie ein Administrator Konto, und wählen Sie dann die Seite mit den **Gutschriften aktualisieren** aus, um zu überprüfen, ob

## <a name="cleaning-up-sql-server-compact-files"></a>Bereinigen von SQL Server Compact Dateien

Dateien und nuget-Pakete, die zur Unterstützung von SQL Server Compact enthalten waren, werden nicht mehr benötigt. Wenn Sie möchten (dieser Schritt ist nicht erforderlich), können Sie nicht benötigte Dateien und Verweise bereinigen.

Löschen Sie in **Projektmappen-Explorer**die *sdf* -Dateien aus dem Ordner *App\_Data* und den *amd64* -und *x86* -Ordnern aus dem Ordner *bin* .

Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf die Projekt Mappe (nicht auf eines der Projekte), und klicken Sie dann auf **nuget-Pakete für**Projekt Mappe verwalten.

Klicken Sie im linken Bereich des Dialog Felds **nuget-Pakete verwalten** auf **installierte Pakete**.

Wählen Sie das Paket **EntityFramework. sqlservercompact** aus, und klicken Sie auf **Verwalten**.

Im Dialogfeld **Projekte auswählen** werden beide Projekte ausgewählt. Wenn Sie das Paket in beiden Projekten deinstallieren möchten, deaktivieren Sie beide Kontrollkästchen, und klicken Sie dann auf **OK**.

Klicken Sie im Dialogfeld, in dem Sie gefragt werden, ob Sie die abhängigen Pakete deinstallieren möchten, auf Nein. Eines davon ist das Entity Framework Paket, das Sie aufbewahren müssen.

Führen Sie das gleiche Verfahren aus, um das **sqlservercompact** -Paket zu deinstallieren. (Die Pakete müssen in dieser Reihenfolge deinstalliert werden, da das Paket " **EntityFramework. sqlservercompact** " vom Paket " **sqlservercompact** " abhängig ist.)

Sie haben nun erfolgreich zu SQL Server Express und vollständiger SQL Server migriert. Im nächsten Tutorial nehmen Sie eine weitere Daten Bank Änderung vor, und Sie erfahren, wie Sie Daten Bank Änderungen bereitstellen, wenn Ihre Test-und Produktionsdatenbanken SQL Server Express und vollständige SQL Server verwenden.

> [!div class="step-by-step"]
> [Zurück](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
> [Weiter](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12.md)
