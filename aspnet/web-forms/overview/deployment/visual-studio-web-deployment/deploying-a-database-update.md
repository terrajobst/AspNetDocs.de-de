---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
title: 'ASP.net-Webbereitstellung mithilfe von Visual Studio: Bereitstellen eines Datenbankupdates | Microsoft-Dokumentation'
author: tdykstra
description: In dieser tutorialreihe erfahren Sie, wie Sie eine ASP.NET-Webanwendung bereitstellen (veröffentlichen), um Web-Apps oder einen Drittanbieter-Hostinganbieter zu Azure App Service.
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 9cad0833-486a-4474-a7f3-7715542ec4ce
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
msc.type: authoredcontent
ms.openlocfilehash: 805eb84c24764cf921291f89054435601dbac48e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78440787"
---
# <a name="aspnet-web-deployment-using-visual-studio-deploying-a-database-update"></a>ASP.net-Webbereitstellung mithilfe von Visual Studio: Bereitstellen eines Datenbankupdates

von [Tom Dykstra](https://github.com/tdykstra)

[Starter Projekt herunterladen](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> In dieser tutorialreihe erfahren Sie, wie Sie eine ASP.NET-Webanwendung bereitstellen (veröffentlichen), um Web-Apps oder einen Drittanbieter-Hostinganbieter mithilfe von Visual Studio 2012 oder Visual Studio 2010 zu Azure App Service. Weitere Informationen zur Reihe finden Sie [im ersten Tutorial der Reihe](introduction.md).

## <a name="overview"></a>Übersicht

In diesem Tutorial erstellen Sie eine Daten Bank Änderung und zugehörige Codeänderungen, testen die Änderungen in Visual Studio und stellen dann das Update für die Test-, Staging-und Produktionsumgebungen bereit.

Das Tutorial zeigt zunächst, wie eine Datenbank aktualisiert wird, die von Code First-Migrationen verwaltet wird, und anschließend wird gezeigt, wie eine Datenbank mithilfe des dbdacfx-Anbieters aktualisiert wird.

Erinnerung: Wenn Sie eine Fehlermeldung erhalten oder etwas nicht funktioniert, wenn Sie das Tutorial durchlaufen, achten Sie darauf, dass Sie die [Seite Problem](troubleshooting.md)Behandlung überprüfen.

## <a name="deploy-a-database-update-by-using-code-first-migrations"></a>Bereitstellen eines Datenbankupdates mithilfe Code First-Migrationen

In diesem Abschnitt fügen Sie der `Person`-Basisklasse für die Entitäten `Student` und `Instructor` eine Geburtsdatums Spalte hinzu. Anschließend aktualisieren Sie die Seite, auf der Dozenten Daten angezeigt werden, damit die neue Spalte angezeigt wird. Zum Schluss stellen Sie die Änderungen in den Test-, Staging-und Produktionsumgebungen bereit.

### <a name="add-a-column-to-a-table-in-the-application-database"></a>Hinzufügen einer Spalte zu einer Tabelle in der Anwendungsdatenbank

1. Öffnen Sie im Projekt *contosouniversity. dal* *Person.cs* , und fügen Sie die folgende Eigenschaft am Ende der `Person` Klasse hinzu (danach müssen zwei schließende geschweifte Klammern vorhanden sein):

    [!code-csharp[Main](deploying-a-database-update/samples/sample1.cs)]

    Aktualisieren Sie als nächstes die `Seed`-Methode, sodass Sie einen Wert für die neue Spalte bereitstellt. Öffnen Sie *migrations\configuration.cs* , und ersetzen Sie den Code Block, der `var instructors = new List<Instructor>` beginnt, durch den folgenden Codeblock, der Geburtsdatum-Informationen enthält:

    [!code-csharp[Main](deploying-a-database-update/samples/sample2.cs)]
2. Erstellen Sie die Projekt Mappe, und öffnen Sie dann das Fenster **Paket-Manager-Konsole** . Stellen Sie sicher, dass "conjesouniversity. dal" weiterhin als **Standard Projekt**ausgewählt ist.
3. Wählen Sie im Fenster **Paket-Manager-Konsole** die Option **condesouniversity. dal** als **Standard Projekt**aus, und geben Sie dann den folgenden Befehl ein:

    [!code-powershell[Main](deploying-a-database-update/samples/sample3.ps1)]

    Wenn dieser Befehl abgeschlossen ist, öffnet Visual Studio die Klassendatei, die die neue `DbMigration`-Klasse definiert, und in der `Up`-Methode sehen Sie den Code, der die neue Spalte erstellt. Die `Up`-Methode erstellt die-Spalte, wenn Sie die Änderung implementieren, und die `Down`-Methode löscht die-Spalte, wenn Sie ein Rollback der Änderung durchführen.

    ![AddBirthDate_migration_code](deploying-a-database-update/_static/image1.png)
4. Erstellen Sie die Projekt Mappe, und geben Sie dann den folgenden Befehl in das Fenster der **Paket-Manager-Konsole** ein (stellen Sie sicher, dass das Projekt condesouniversity. dal noch ausgewählt ist):

    [!code-powershell[Main](deploying-a-database-update/samples/sample4.ps1)]

    Der Entity Framework führt die `Up`-Methode aus und führt dann die `Seed`-Methode aus.

### <a name="display-the-new-column-in-the-instructors-page"></a>Anzeigen der neuen Spalte auf der Seite "Dozenten"

1. Öffnen Sie im Projekt contosouniversity die Datei " *Dozenten. aspx* ", und fügen Sie ein neues Vorlagen Feld hinzu, um das Geburtsdatum anzuzeigen. Fügen Sie diese zwischen den Einstellungen für das Einstellungs Datum und die Büro Zuweisung hinzu:

    [!code-aspx[Main](deploying-a-database-update/samples/sample5.aspx?highlight=9-17)]

    (Wenn der Code Einzug nicht synchronisiert wird, können Sie STRG + K drücken und dann STRG + D drücken, um die Datei automatisch neu zu formatieren.)
2. Führen Sie die Anwendung aus, und klicken Sie auf den Link **Dozenten** .

    Wenn die Seite geladen wird, sehen Sie, dass Sie das neue Feld Geburtsdatum enthält.

    ![Dozenten Seite mit BirthDate](deploying-a-database-update/_static/image2.png)
3. Schließen Sie den Browser.

### <a name="deploy-the-database-update"></a>Datenbankupdate bereitstellen

1. Wählen Sie in **Projektmappen-Explorer** das Projekt conjesouniversity aus.
2. Klicken Sie im **Web** auf die Symbolleiste veröffentlichen, klicken Sie **auf das Profil** Veröffentlichungs Profil, und klicken Sie dann auf **Web veröffentlichen**. (Wenn die Symbolleiste deaktiviert ist, wählen Sie das Projekt condesouniversity in **Projektmappen-Explorer**aus.)

    Visual Studio stellt die aktualisierte Anwendung bereit, und der Browser wird mit der Startseite geöffnet.
3. Führen Sie die Seite **Dozenten** aus, um sicherzustellen, dass das Update erfolgreich bereitgestellt wurde.

    Wenn die Anwendung versucht, auf die Datenbank für diese Seite zuzugreifen, aktualisiert Code First das Datenbankschema und führt die `Seed`-Methode aus. Wenn die Seite angezeigt wird, wird die Spalte erwarteter **Geburtsdatum** mit Datumsangaben angezeigt.
4. Klicken Sie im **Web** auf die Symbolleiste veröffentlichen, klicken Sie **auf das** stagingveröffentlichungsprofil, und klicken Sie dann auf **Web veröffentlichen**.
5. Führen Sie die Seite **Dozenten** im Staging aus, um zu überprüfen, ob das Update erfolgreich bereitgestellt wurde.
6. Klicken Sie im Web auf die Symbolleiste **veröffentlichen** , klicken Sie auf das Veröffentlichungs Profil für die **Produktion** , und klicken Sie dann auf **Web veröffentlichen**.
7. Führen Sie die Seite **Dozenten** in der Produktion aus, um sicherzustellen, dass das Update erfolgreich bereitgestellt wurde.

    Bei einem realen Produktions Anwendungs Update, das eine Daten Bank Änderung umfasst, würden Sie die Anwendung normalerweise während der Bereitstellung offline schalten, indem Sie *App\_offline. htm*verwenden, wie im vorherigen Tutorial gezeigt.

## <a name="deploy-a-database-update-by-using-the-dbdacfx-provider"></a>Bereitstellen eines Datenbankupdates mithilfe des dbdacfx-Anbieters

In diesem Abschnitt fügen Sie der *Benutzer* Tabelle in der Mitgliedschafts Datenbank eine Spalte " *Kommentare* " hinzu und erstellen eine Seite, auf der Sie Kommentare für jeden Benutzer anzeigen und bearbeiten können. Anschließend stellen Sie die Änderungen in den Test-, Staging-und Produktionsumgebungen bereit.

### <a name="add-a-column-to-a-table-in-the-membership-database"></a>Hinzufügen einer Spalte zu einer Tabelle in der Mitgliedschafts Datenbank

1. Öffnen Sie in Visual Studio **SQL Server-Objekt-Explorer**.
2. Erweitern Sie **(localdb) \v11.0**, erweitern Sie **Datenbanken**, erweitern Sie **ASPNET-contosouniversity** (nicht **ASPNET-contosouniversity-Prod**), und erweitern Sie dann **Tabellen**.

    Wenn **(localdb) \v11.0** unter dem Knoten **SQL Server** nicht angezeigt wird, klicken Sie mit der rechten Maustaste auf den Knoten **SQL Server** , und klicken Sie dann auf **SQL Server hinzufügen**. Geben Sie im Dialogfeld **Verbindung mit Server herstellen** *(localdb) \v11.0* als **Server Namen**ein, und klicken Sie dann auf **verbinden**.

    Wenn **ASPNET-contosouniversity**nicht angezeigt wird, führen Sie das Projekt aus, und melden Sie sich mit den *Administrator* Anmelde Informationen (Kennwort ist *devpwd*) an, und aktualisieren Sie dann das Fenster **SQL Server-Objekt-Explorer** .
3. Klicken Sie mit der rechten Maustaste auf die Tabelle **Benutzer** , und klicken Sie dann auf **Designer anzeigen**.

    ![Ssox-Sicht-Designer](deploying-a-database-update/_static/image3.png)
4. Fügen Sie im Designer eine Spalte " *Kommentare* " hinzu, und legen Sie **Sie auf "** *nvarchar (128)* " und "Nullable" fest.

    ![Hinzufügen von Kommentarspalten](deploying-a-database-update/_static/image4.png)
5. Klicken Sie im Feld **Vorschau der Daten Bank Updates** auf **Datenbank aktualisieren**.

    ![Vorschau der Daten Bank Updates](deploying-a-database-update/_static/image5.png)

### <a name="create-a-page-to-display-and-edit-the-new-column"></a>Erstellen einer Seite zum Anzeigen und Bearbeiten der neuen Spalte

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den **Konto** Ordner im Projekt contosouniversity, klicken Sie auf **Hinzufügen**, und klicken Sie dann auf **Neues Element**.
2. Erstellen Sie ein neues **Webformular mithilfe der Master Seite** , und nennen Sie es " *userinfo. aspx*". Akzeptieren Sie die Standarddatei " *Site. Master* " als Master Seite.
3. Kopieren Sie das folgende Markup in das `MainContent` `Content`-Element (die letzten 3 `Content` Elemente):

    [!code-aspx[Main](deploying-a-database-update/samples/sample6.aspx)]
4. Klicken Sie mit der rechten Maustaste auf die Seite *userinfo. aspx* , und klicken Sie auf **in Browser anzeigen**.
5. Melden Sie sich mit Ihren *Administrator* Anmelde Informationen an (Kennwort ist *devpwd*), und fügen Sie einem Benutzer einige Kommentare hinzu, um zu überprüfen, ob die Seite ordnungsgemäß funktioniert.

    ![Userinfo-Seite](deploying-a-database-update/_static/image6.png)
6. Schließen Sie den Browser.

## <a name="deploy-the-database-update"></a>Datenbankupdate bereitstellen

Zum Bereitstellen von mithilfe des dbdacfx-Anbieters müssen Sie nur die Option **Datenbank aktualisieren** im Veröffentlichungs Profil auswählen. Allerdings haben Sie für die erste Bereitstellung, bei der Sie diese Option verwendet haben, auch einige zusätzliche SQL-Skripts für die Ausführung konfiguriert: Diese befinden sich noch im Profil, und Sie müssen verhindern, dass Sie erneut ausgeführt werden.

1. Öffnen Sie den Assistenten **Web veröffentlichen** , indem Sie mit der rechten Maustaste auf das Projekt contosouniversity und dann auf **veröffentlichen**klicken.
2. Wählen Sie das **Test** Profil aus.
3. Klicken Sie auf die Registerkarte **Settings** .
4. Wählen Sie unter **DefaultConnection**die Option **Datenbank aktualisieren**aus.
5. Deaktivieren Sie die zusätzlichen Skripts, die Sie für die erste Bereitstellung konfiguriert haben:

    1. Klicken Sie auf **Datenbankupdates konfigurieren**.
    2. Deaktivieren Sie im Dialogfeld **Daten Bank Updates konfigurieren** die Kontrollkästchen neben *Grant. SQL* und *ASPNET-Data-dev. SQL*.
    3. Klicken Sie auf **Schließen**.
6. Klicken Sie auf die Registerkarte **Vorschau** .
7. Klicken Sie unter **Datenbanken** und rechts neben **DefaultConnection**auf den Link **Datenbank anzeigen** .

    ![Datenbank-Vorschau](deploying-a-database-update/_static/image7.png)

    Im Vorschaufenster wird das Skript angezeigt, das in der Zieldatenbank ausgeführt wird, um zu erreichen, dass das Datenbankschema dem Schema der Quelldatenbank entspricht. Das Skript enthält einen ALTER TABLE-Befehl, der die neue Spalte hinzufügt.
8. Schließen Sie das Dialogfeld **Datenbank-Vorschau** , und klicken Sie dann auf **veröffentlichen**.

    Visual Studio stellt die aktualisierte Anwendung bereit, und der Browser wird mit der Startseite geöffnet.
9. Führen Sie die userinfo-Seite ( *Konto/userinfo. aspx* zur URL der Startseite hinzufügen) aus, um zu überprüfen, ob das Update erfolgreich bereitgestellt wurde. Sie müssen sich anmelden, indem Sie *Admin* und *devpwd*eingeben.

    Daten in Tabellen werden nicht standardmäßig bereitgestellt, und Sie haben kein Skript für die Datenbereitstellung für die Durchführung konfiguriert, sodass Sie den Kommentar nicht finden, den Sie in der Entwicklung hinzugefügt haben. Sie können jetzt einen neuen Kommentar in der Stagingumgebung hinzufügen, um sicherzustellen, dass die Änderung für die Datenbank bereitgestellt wurde und die Seite ordnungsgemäß funktioniert.
10. Befolgen Sie das gleiche Verfahren für die Bereitstellung für Staging und Produktion.

    Vergessen Sie nicht, die zusätzlichen Skripts zu deaktivieren. Der einzige Unterschied im Vergleich zum Test Profil besteht darin, dass Sie nur ein Skript in den Staging-und Produktions Profilen deaktivieren, da Sie so konfiguriert wurden, dass nur *ASPNET-Prod-Data. SQL*ausgeführt wird.

    Die Anmelde Informationen für Staging und Produktion lauten admin und prodpwd.

    Bei einem realen Produktions Anwendungs Update, das eine Daten Bank Änderung umfasst, würden Sie die Anwendung normalerweise während der Bereitstellung offline schalten, indem Sie *App-\_offline. htm* hochladen, bevor Sie Sie veröffentlichen und anschließend löschen, wie im [vorherigen Tutorial](deploying-a-code-update.md)gezeigt.

## <a name="summary"></a>Zusammenfassung

Sie haben nun ein Anwendungs Update bereitgestellt, das eine Daten Bank Änderung mithilfe Code First-Migrationen und des dbdacfx-Anbieters enthielt.

![Dozenten Seite mit BirthDate](deploying-a-database-update/_static/image8.png)

![Userinfo-Seite](deploying-a-database-update/_static/image9.png)

Im nächsten Tutorial erfahren Sie, wie Sie bereit Stellungen über die Befehlszeile ausführen.

> [!div class="step-by-step"]
> [Zurück](deploying-a-code-update.md)
> [Weiter](command-line-deployment.md)
