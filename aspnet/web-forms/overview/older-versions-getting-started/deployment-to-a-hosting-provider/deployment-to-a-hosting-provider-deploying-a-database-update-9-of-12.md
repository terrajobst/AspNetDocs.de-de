---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
title: 'Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mithilfe von Visual Studio oder Visual Web Developer: Bereitstellen eines Datenbankupdates-9 von 12 | Microsoft-Dokumentation'
author: tdykstra
description: In dieser Reihe von Tutorials wird gezeigt, wie Sie ein ASP.NET-Webanwendungs Projekt bereitstellen (veröffentlichen), das eine SQL Server Compact-Datenbank mit Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: a8d776af-4735-4612-87f6-9f326587f2d3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
msc.type: authoredcontent
ms.openlocfilehash: 3385e1019d9e7a9263fd9513c39b25eb85febbb5
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74582021"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-database-update---9-of-12"></a>Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mithilfe von Visual Studio oder Visual Web Developer: Bereitstellen eines Datenbankupdates-9 von 12

von [Tom Dykstra](https://github.com/tdykstra)

[Starter Projekt herunterladen](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> In dieser Reihe von Tutorials erfahren Sie, wie Sie ein ASP.NET-Webanwendungs Projekt, das eine SQL Server Compact Datenbank enthält, mithilfe von Visual Studio 2012 RC oder Visual Studio Express 2012 RC für das Web bereitstellen (veröffentlichen). Sie können auch Visual Studio 2010 verwenden, wenn Sie das Webveröffentlichungs Update installieren. Eine Einführung in die Reihe finden Sie [im ersten Tutorial der Reihe](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Ein Tutorial, das nach der RC-Version von Visual Studio 2012 eingeführte Bereitstellungs Funktionen zeigt, zeigt, wie SQL Server Editionen außer SQL Server Compact bereitgestellt werden, und zeigt, wie Sie die Bereitstellung für Azure App Service Web-Apps ausführen. Weitere Informationen finden Sie unter [ASP.net Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Übersicht über

In diesem Tutorial erstellen Sie eine Daten Bank Änderung und zugehörige Codeänderungen, testen die Änderungen in Visual Studio und stellen das Update dann sowohl in der Test-als auch in der Produktionsumgebung bereit.

Erinnerung: Wenn Sie eine Fehlermeldung erhalten oder etwas nicht funktioniert, wenn Sie das Tutorial durchlaufen, achten Sie darauf, dass Sie die [Seite Problem](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)Behandlung überprüfen.

## <a name="adding-a-new-column-to-a-table"></a>Hinzufügen einer neuen Spalte zu einer Tabelle

In diesem Abschnitt fügen Sie der `Person`-Basisklasse für die Entitäten `Student` und `Instructor` eine Geburtsdatums Spalte hinzu. Anschließend aktualisieren Sie die Seite, auf der Dozenten Daten angezeigt werden, damit die neue Spalte angezeigt wird.

Öffnen Sie im Projekt *contosouniversity. dal* *Person.cs* , und fügen Sie die folgende Eigenschaft am Ende der `Person` Klasse hinzu (danach müssen zwei schließende geschweifte Klammern vorhanden sein):

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample1.cs)]

Aktualisieren Sie anschließend die Seed-Methode, sodass Sie einen Wert für die neue Spalte bereitstellt. Öffnen Sie *migrations\configuration.cs* , und ersetzen Sie den Code Block, der `var instructors = new List<Instructor>` beginnt, durch den folgenden Codeblock, der Geburtsdatum-Informationen enthält:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample2.cs)]

Öffnen Sie im Projekt contosouniversity die Datei " *Dozenten. aspx* ", und fügen Sie ein neues Vorlagen Feld hinzu, um das Geburtsdatum anzuzeigen. Fügen Sie diese zwischen den Einstellungen für das Einstellungs Datum und die Büro Zuweisung hinzu:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample3.aspx)]

(Wenn der Code Einzug nicht synchronisiert wird, können Sie STRG + K drücken und dann STRG + D drücken, um die Datei automatisch neu zu formatieren.)

Erstellen Sie die Projekt Mappe, und öffnen Sie dann das Fenster **Paket-Manager-Konsole** . Stellen Sie sicher, dass "conjesouniversity. dal" weiterhin als **Standard Projekt**ausgewählt ist.

Wählen Sie im Fenster **Paket-Manager-Konsole** die Option **condesouniversity. dal** als **Standard Projekt**aus, und geben Sie dann den folgenden Befehl ein:

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample4.ps1)]

Wenn dieser Befehl abgeschlossen ist, öffnet Visual Studio die Klassendatei, die die neue `DbMigration`-Klasse definiert, und in der `Up`-Methode sehen Sie den Code, der die neue Spalte erstellt.

![AddBirthDate_migration_code](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image1.png)

Erstellen Sie die Projekt Mappe, und geben Sie dann den folgenden Befehl in das Fenster der **Paket-Manager-Konsole** ein (stellen Sie sicher, dass das Projekt condesouniversity. dal noch ausgewählt ist):

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample5.ps1)]

Wenn der Befehl abgeschlossen ist, führen Sie die Anwendung aus, und wählen Sie die Seite Dozenten aus. Wenn die Seite geladen wird, sehen Sie, dass Sie das neue Feld Geburtsdatum enthält.

[![Instructors_page_with_birth_date](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image3.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image2.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>Bereitstellen des Datenbankupdates in der Test Umgebung

Wählen Sie in **Projektmappen-Explorer** das Projekt conjesouniversity aus.

Klicken Sie im **Internet auf** die Symbolleiste veröffentlichen, wählen **Sie das Profil Veröffentlichungs Profil** aus, und klicken Sie dann auf **Web veröffentlichen**. (Wenn die Symbolleiste deaktiviert ist, wählen Sie das Projekt condesouniversity in **Projektmappen-Explorer**aus.)

Visual Studio stellt die aktualisierte Anwendung bereit, und der Browser wird mit der Startseite geöffnet. Führen Sie die Seite Dozenten aus, um sicherzustellen, dass das Update erfolgreich bereitgestellt wurde. Wenn die Anwendung versucht, auf die Datenbank für diese Seite zuzugreifen, aktualisiert Code First das Datenbankschema und führt die `Seed`-Methode aus. Wenn die Seite angezeigt wird, wird die Spalte erwarteter **Geburtsdatum** mit Datumsangaben angezeigt.

[![Instructors_page_with_birth_date_Test](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image4.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>Bereitstellen des Datenbankupdates in der Produktionsumgebung

Sie können jetzt in der Produktionsumgebung bereitstellen. Der einzige Unterschied besteht darin, dass Sie *App\_offline. htm* verwenden, um zu verhindern, dass Benutzer auf die Website zugreifen und die Datenbank aktualisieren, während Sie Änderungen bereitstellen. Führen Sie für die Produktions Bereitstellung die folgenden Schritte aus:

- Laden Sie die *App\_Datei "offline. htm* " auf die Produktions Site hoch.
- Wählen Sie in Visual Studio das Produktionsprofil im Web aus, und klicken Sie auf **veröffentlichen** Symbolleiste, und klicken Sie dann auf **Web veröffentlichen**.
- Löschen Sie die *App\_Datei "offline. htm* " aus der Produktions Website.

> [!NOTE]
> Während Ihre Anwendung in der Produktionsumgebung verwendet wird, sollten Sie einen Sicherungs Plan implementieren. Das heißt, dass Sie in regelmäßigen Abständen die Dateien *School-Prod. sdf* und *ASPNET-Prod. sdf* von der Produktions Website an einen sicheren Speicherort kopieren müssen, und Sie sollten mehrere Generationen solcher Sicherungen aufbewahren. Wenn Sie die Datenbank aktualisieren, sollten Sie direkt vor der Änderung eine Sicherungskopie erstellen. Wenn Sie dann einen Fehler machen und ihn erst ermitteln, wenn Sie ihn in der Produktionsumgebung bereitgestellt haben, können Sie die Datenbank weiterhin in dem Zustand wiederherstellen, in dem Sie sich befand, bevor Sie beschädigt wurde.

Wenn Visual Studio die URL der Startseite im Browser öffnet, wird die Seite *App\_offline. htm* angezeigt. Nachdem Sie die *App\_Datei "offline. htm* " gelöscht haben, können Sie erneut zu Ihrer Startseite navigieren, um zu überprüfen, ob das Update erfolgreich bereitgestellt wurde.

[![Instructors_page_with_birth_date_Prod](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image7.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image6.png)

Sie haben nun ein Anwendungs Update bereitgestellt, das eine Daten Bank Änderung in Test-und Produktionsumgebungen enthielt. Im nächsten Tutorial erfahren Sie, wie Sie Ihre Datenbank von SQL Server Compact zu SQL Server Express und SQL Server migrieren.

> [!div class="step-by-step"]
> [Zurück](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
> [Weiter](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
