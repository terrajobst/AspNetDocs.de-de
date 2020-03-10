---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
title: 'Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mithilfe von Visual Studio oder Visual Web Developer: Bereitstellen eines SQL Server Datenbankupdates-11 von 12 | Microsoft-Dokumentation'
author: tdykstra
description: In dieser Reihe von Tutorials wird gezeigt, wie Sie ein ASP.NET-Webanwendungs Projekt bereitstellen (veröffentlichen), das eine SQL Server Compact-Datenbank mit Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: 5e2bb092-cb22-4511-ad0a-22ae12dd99b3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
msc.type: authoredcontent
ms.openlocfilehash: 0894c0ac24737e66b6960ef3d48aa17f78c6aa1d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78423975"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-sql-server-database-update---11-of-12"></a>Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mithilfe von Visual Studio oder Visual Web Developer: Bereitstellen eines SQL Server Datenbankupdates-11 von 12

von [Tom Dykstra](https://github.com/tdykstra)

[Starter Projekt herunterladen](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> In dieser Reihe von Tutorials erfahren Sie, wie Sie ein ASP.NET-Webanwendungs Projekt, das eine SQL Server Compact Datenbank enthält, mithilfe von Visual Studio 2012 RC oder Visual Studio Express 2012 RC für das Web bereitstellen (veröffentlichen). Sie können auch Visual Studio 2010 verwenden, wenn Sie das Webveröffentlichungs Update installieren. Eine Einführung in die Reihe finden Sie [im ersten Tutorial der Reihe](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Ein Lernprogramm, das nach der RC-Version von Visual Studio 2012 eingeführte Bereitstellungs Funktionen zeigt, zeigt, wie SQL Server Editionen außer SQL Server Compact bereitgestellt werden, und zeigt, wie Sie die Bereitstellung auf Windows Azure-Websites durchführt. Weitere Informationen finden Sie unter [ASP.net Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Übersicht

In diesem Tutorial wird gezeigt, wie Sie ein Datenbankupdate in einer vollständigen SQL Server Datenbank bereitstellen. Da Code First-Migrationen den gesamten Arbeitsaufwand für das Aktualisieren der Datenbank erledigt, ist der Prozess fast identisch mit dem, was Sie im Tutorial SQL Server Compact zum Bereitstellen [eines Datenbankupdates](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) durchgeführt haben.

Erinnerung: Wenn Sie eine Fehlermeldung erhalten oder etwas nicht funktioniert, wenn Sie das Tutorial durchlaufen, achten Sie darauf, dass Sie die [Seite Problem](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)Behandlung überprüfen.

## <a name="adding-a-new-column-to-a-table"></a>Hinzufügen einer neuen Spalte zu einer Tabelle

In diesem Abschnitt des Tutorials erstellen Sie eine Daten Bank Änderung und entsprechende Codeänderungen und testen Sie dann in Visual Studio, um Sie in den Test-und Produktionsumgebungen bereitzustellen. Die Änderung umfasst das Hinzufügen einer `OfficeHours` Spalte zur `Instructor` Entität und das Anzeigen der neuen Informationen auf der Seite **Dozenten** .

Öffnen Sie im Projekt contosouniversity. dal *Instructor.cs* , und fügen Sie die folgende Eigenschaft zwischen den Eigenschaften `HireDate` und `Courses` hinzu:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample1.cs)]

Aktualisieren Sie die Initialisiererklasse, sodass Sie die neue Spalte mit den Testdaten startet. Öffnen Sie *migrations\configuration.cs* , und ersetzen Sie den Codeblock, der `var instructors = new List<Instructor>` beginnt, durch den folgenden Codeblock, der die neue Spalte enthält:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample2.cs)]

Öffnen Sie im Projekt contosouniversity die Datei " *Dozenten. aspx* ", und fügen Sie ein neues Vorlagen Feld für Office-Stunden direkt vor dem schließenden `</Columns>`-Tag im ersten `GridView`-Steuerelement hinzu:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample3.aspx)]

Erstellen Sie die Projektmappe.

Öffnen Sie das Fenster der **Paket-Manager-Konsole** , und wählen Sie contosouniversity. dal als **Standard Projekt**aus.

Geben Sie die folgenden Befehle ein:

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample4.ps1)]

Führen Sie die Anwendung aus, und wählen Sie die Seite **Dozenten** aus. Das Laden der Seite dauert etwas länger als üblich, da der Entity Framework die Datenbank neu erstellt und Sie mit Testdaten erstellt.

[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>Bereitstellen des Datenbankupdates in der Test Umgebung

Wenn Sie Code First-Migrationen verwenden, ist die Methode zum Bereitstellen einer Daten Bank Änderung in SQL Server identisch mit der für SQL Server Compact. Allerdings müssen Sie das Profil für die Test Veröffentlichung ändern, da es noch für die Migration von SQL Server Compact zu SQL Server eingerichtet ist.

Der erste Schritt besteht darin, die Verbindungs Zeichenfolgen-Transformationen zu entfernen, die Sie im vorherigen Tutorial erstellt haben. Diese werden nicht mehr benötigt, da Sie Verbindungs Zeichenfolgen-Transformationen im Veröffentlichungs Profil angeben, wie Sie dies vor der Konfiguration der Registerkarte " **packen/veröffentlichen** " für die Migration zu SQL Server durchgeführt haben.

Öffnen Sie die Datei *Web. Test. config* , und entfernen Sie das `connectionStrings` Element. Die einzige verbleibende Transformation in der Datei " *Web. Test. config* " ist für den `Environment` Wert im `appSettings`-Element vorgesehen.

Nun können Sie das Veröffentlichungs Profil aktualisieren und in der Testumgebung veröffentlichen.

Öffnen Sie den Assistenten **Web veröffentlichen** , und wechseln Sie dann zur Registerkarte **Profil** .

Wählen Sie das Profil für die **Test** Veröffentlichung aus.

Wählen Sie die Registerkarte **Einstellungen** aus.

Klicken Sie auf **neue Verbesserungen der Daten Bank Veröffentlichung aktivieren**

Geben Sie im Feld Verbindungs Zeichenfolge für **schoolContext**denselben Wert ein, den Sie in der Transformations Datei *Web. Test. config* im vorherigen Tutorial verwendet haben:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample5.cmd)]

Wählen Sie **Code First-Migrationen ausführen (wird beim Anwendungsstart ausgeführt) aus**. (In Ihrer Version von Visual Studio kann das Kontrollkästchen mit der Bezeichnung **Apply Code First-Migrationen**bezeichnet werden.)

Geben Sie im Feld Verbindungs Zeichenfolge für **DefaultConnection**den gleichen Wert ein, den Sie in der Transformations Datei *Web. Test. config* im vorherigen Tutorial verwendet haben:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample6.cmd)]

Lassen Sie **Datenbank aktualisieren** deaktiviert.

Klicken Sie auf **Veröffentlichen**.

Visual Studio stellt die Codeänderungen in der Testumgebung bereit und öffnet den Browser auf der Homepage der "Visual Studio"-Homepage.

Wählen Sie die Seite Dozenten aus.

Wenn diese Seite von der Anwendung ausgeführt wird, wird versucht, auf die Datenbank zuzugreifen. Code First-Migrationen überprüft, ob die Datenbank aktuell ist, und stellt fest, dass die aktuelle Migration noch nicht angewendet wurde. Code First-Migrationen wendet die neueste Migration an, führt die `Seed`-Methode aus, und die Seite wird normal ausgeführt. Die neue Spalte "Office-Stunden" wird mit den Seeding Daten angezeigt.

[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>Bereitstellen des Datenbankupdates in der Produktionsumgebung

Sie müssen auch das Veröffentlichungs Profil für die Produktionsumgebung ändern. In diesem Fall entfernen Sie das vorhandene Profil, und erstellen Sie ein neues, indem Sie eine aktualisierte publishsettings-Datei importieren. Die aktualisierte Datei enthält die Verbindungs Zeichenfolge für die SQL Server-Datenbank bei cytanium.

Wie Sie gesehen haben, als Sie in der Testumgebung bereitgestellt haben, benötigen Sie keine Verbindungs Zeichenfolgen-Transformationen mehr in der Transformations Datei *Web. Production. config* . Öffnen Sie diese Datei, und entfernen Sie das `connectionStrings` Element. Die restlichen Transformationen gelten für den `Environment` Wert im `appSettings`-Element und das `location`-Element, das den Zugriff auf ELMAH-Fehlerberichte einschränkt.

Bevor Sie ein neues Veröffentlichungs Profil für die Produktion erstellen, laden Sie eine aktualisierte publishsettings-Datei auf die gleiche Weise wie zuvor im Tutorial bereitstellen [in der Produktionsumgebung](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) . (Klicken Sie in der Systemsteuerung cytanium auf **Websites**, und klicken Sie dann auf die Website **contosouniversity.com** . Wählen Sie die Registerkarte **Webpublishing** aus, und klicken Sie dann auf **Veröffentlichungs Profil für diese Website herunterladen**.) Der Grund hierfür ist, dass Sie die Datenbank-Verbindungs Zeichenfolge in der publishsettings-Datei abrufen. Die Verbindungs Zeichenfolge war nicht verfügbar, als Sie die Datei zum ersten mal heruntergeladen haben, weil Sie noch SQL Server Compact verwendet haben und die SQL Server Datenbank noch nicht bei cytanium erstellt haben.

Nun können Sie das Veröffentlichungs Profil aktualisieren und in der Produktionsumgebung veröffentlichen.

Öffnen Sie den Assistenten **Web veröffentlichen** , und wechseln Sie dann zur Registerkarte **Profil** .

Klicken Sie auf **Profile verwalten**, und löschen Sie dann das Produktionsprofil.

Schließen Sie den Assistenten **Web veröffentlichen** , um diese Änderung zu speichern.

Öffnen Sie den Assistenten **Web veröffentlichen** erneut, und klicken Sie dann auf **importieren**.

Ändern Sie auf der Registerkarte **Verbindung** die **Ziel-URL** in den entsprechenden Wert, wenn Sie eine temporäre URL verwenden.

Klicken Sie auf **Weiter**.

Klicken Sie auf der Registerkarte **Einstellungen** auf **neue Verbesserungen bei der Daten Bank Veröffentlichung aktivieren**.

Wählen Sie in der Dropdown Liste Verbindungs Zeichenfolge für **schoolContext**die Verbindungs Zeichenfolge cytanium aus.

![Selecting_Cytanium_connection_string](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image5.png)

Wählen Sie **Code First Migrationen ausführen (wird beim Anwendungsstart ausgeführt) aus**.

Wählen Sie in der Dropdown Liste Verbindungs Zeichenfolge für **DefaultConnection**die Verbindungs Zeichenfolge cytanium aus.

Wählen Sie die Registerkarte **Profil** aus, klicken Sie auf **Profile verwalten**, und benennen Sie das Profil aus "contosouniversity.com-Web deploy" in "Production" um.

Schließen Sie das Veröffentlichungs Profil, um die Änderung zu speichern, und öffnen Sie Sie dann erneut.

Klicken Sie auf **Veröffentlichen**. (Bei einer realen Produktions Website würden Sie die *App\_"offline. htm* " in die Produktionsumgebung kopieren und vor der Veröffentlichung in Ihren Projektordner einfügen und dann nach Abschluss der Bereitstellung entfernen.)

Visual Studio stellt die Codeänderungen in der Testumgebung bereit und öffnet den Browser auf der Homepage der "Visual Studio"-Homepage.

Wählen Sie die Seite Dozenten aus.

Code First-Migrationen aktualisiert die Datenbank auf die gleiche Weise wie in der Test Umgebung. Die neue Spalte "Office-Stunden" wird mit den Seeding Daten angezeigt.

![Instructors_page_with_OfficeHours_Prod](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image6.png)

Sie haben nun erfolgreich ein Anwendungs Update bereitgestellt, das eine Daten Bank Änderung mit einer SQL Server Datenbank enthielt.

## <a name="more-information"></a>Weitere Informationen

Dies schließt diese Reihe von Tutorials zum Bereitstellen einer ASP.NET-Webanwendung für einen Drittanbieter-Hostinganbieter ab. Weitere Informationen zu den Themen, die in diesen Tutorials behandelt werden, finden Sie auf der MSDN-Website in der [ASP.net-Bereitstellungs Inhalts Karte](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx) .

## <a name="acknowledgements"></a>Danksagung

Ich möchte den folgenden Personen danken, die bedeutende Beiträge zum Inhalt dieser tutorialreihe gemacht haben:

- [Alberto Poblacion, MVP &amp; MCT, Spanien](https://mvp.support.microsoft.com/profile/Alberto)
- Jarod Ferguson, Data Platform Development MVP, USA
- Harte Verpflichtung, Microsoft
- [Kristina Olson, Microsoft](https://blogs.iis.net/krolson/default.aspx)
- [Mike Papst, Microsoft](http://www.mikepope.com/blog/DisplayBlog.aspx)
- Treffen Sie Srivastava, Microsoft
- [Raffaele rialdi, Italien](http://www.iamraf.net/)
- [Rick Anderson, Microsoft](https://blogs.msdn.com/b/rickandy/)
- [Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(Twitter: [@sayedihashimi](http://twitter.com/sayedihashimi))
- [Scott Hanselman](http://www.hanselman.com/blog/) (Twitter: [@shanselman](http://twitter.com/shanselman))
- [Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (Twitter: [@coolcsh](http://twitter.com/coolcsh))
- [Srđan Božović, Serbien](http://msforge.net/blogs/zmajcek/)
- [Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (Twitter: [@vishalrjoshi](http://twitter.com/vishalrjoshi))

> [!div class="step-by-step"]
> [Zurück](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
> [Weiter](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)
