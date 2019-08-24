---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Tutorial: Verwenden von EF-Migrationen in einer ASP.NET MVC-APP und Bereitstellen in Azure'
author: tdykstra
description: In diesem Tutorial aktivieren Sie Code First Migrationen und stellen die Anwendung in der Cloud in Azure bereit.
ms.author: riande
ms.date: 01/16/2019
ms.topic: tutorial
ms.assetid: d4dfc435-bda6-4621-9762-9ba270f8de4e
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 989dd0f0e18b338be057b9c5657586eff996d8ea
ms.sourcegitcommit: b95316530fa51087d6c400ff91814fe37e73f7e8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/23/2019
ms.locfileid: "70000769"
---
# <a name="tutorial-use-ef-migrations-in-an-aspnet-mvc-app-and-deploy-to-azure"></a>Tutorial: Verwenden von EF-Migrationen in einer ASP.NET MVC-APP und Bereitstellen in Azure

Bisher wurde die Beispiel-Webanwendung der Beispiel-Web-App lokal in IIS Express auf Ihrem Entwicklungs Computer ausgeführt. Um eine echte Anwendung für andere Personen über das Internet zur Verfügung zu stellen, müssen Sie Sie für einen Webhostinganbieter bereitstellen. In diesem Tutorial aktivieren Sie Code First Migrationen und stellen die Anwendung in der Cloud in Azure bereit:

- Aktivieren Sie Code First-Migrationen. Mithilfe der Migrations Funktion können Sie das Datenmodell ändern und die Änderungen in der Produktionsumgebung bereitstellen, indem Sie das Datenbankschema aktualisieren, ohne die Datenbank löschen und neu erstellen zu müssen.
- In Azure bereitstellen. Dieser Schritt ist optional. Sie können mit den verbleibenden Tutorials fortfahren, ohne das Projekt bereitzustellen.

Es wird empfohlen, dass Sie einen Continuous Integration Prozess mit der Quell Code Verwaltung für die Bereitstellung verwenden, aber dieses Tutorial behandelt diese Themen nicht. Weitere Informationen finden Sie in der [Quell](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control) Code Verwaltung und in den [Continuous Integration](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery) Kapiteln zum entwickeln [realer Cloud-apps mit Azure](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction).

In diesem Tutorial:

> [!div class="checklist"]
> * Code First Migrationen aktivieren
> * Bereitstellen der app in Azure (optional)

## <a name="prerequisites"></a>Erforderliche Komponenten

- [Verbindungsresilienz und Abfangen von Befehlen](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="enable-code-first-migrations"></a>Code First Migrationen aktivieren

Wenn Sie eine neue Anwendung entwickeln, ändert sich Ihr Datenmodell häufig. Jedes Mal, wenn das Datenmodell geändert wird, ist es nicht mehr synchron mit der Datenbank. Sie haben die Entity Framework so konfiguriert, dass die Datenbank jedes Mal, wenn Sie das Datenmodell ändern, automatisch gelöscht und neu erstellt wird. Wenn Sie Entitäts Klassen hinzufügen, entfernen oder ändern oder die `DbContext` Klasse ändern, wird beim nächsten Ausführen der Anwendung automatisch die vorhandene Datenbank gelöscht, eine neue erstellt, die mit dem Modell übereinstimmt, und Sie wird mit Testdaten versehen.

Diese Methode, bei der die Datenbank mit dem Datenmodell synchron gehalten wird, funktioniert so lange, bis Sie die Anwendung für die Produktion bereitstellen. Wenn die Anwendung in der Produktionsumgebung ausgeführt wird, speichert Sie in der Regel Daten, die Sie behalten möchten, und Sie möchten nicht jedes Mal alles verlieren, wenn Sie eine Änderung vornehmen, wie z. b. das Hinzufügen einer neuen Spalte. Das [Code First-Migrationen](https://msdn.microsoft.com/data/jj591621) Feature löst dieses Problem, indem es Code First das Aktualisieren des Datenbankschemas ermöglicht, anstatt die Datenbank zu löschen und neu zu erstellen. In diesem Tutorial stellen Sie die Anwendung bereit, um die Migration zu ermöglichen.

1. Deaktivieren Sie den zuvor eingerichteten Initialisierer, indem Sie das `contexts` Element, das Sie der Web. config-Datei der Anwendung hinzugefügt haben, auskommentieren oder löschen.

    [!code-xml[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.xml?highlight=2,6)]
2. Ändern Sie in der Datei " *Web. config* " der Anwendung auch den Namen der Datenbank in der Verbindungs Zeichenfolge in "contosouniversity2".

    [!code-xml[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.xml?highlight=2)]

    Durch diese Änderung wird das Projekt so eingerichtet, dass bei der ersten Migration eine neue Datenbank erstellt wird. Dies ist nicht erforderlich, aber Sie werden später sehen, warum es eine gute Idee ist.
3. Öffnen Sie das Menü **Extras**, und wählen Sie **NuGet-Paket-Manager** > **Paket-Manager-Konsole** aus.

1. Geben Sie `PM>` an der Eingabeaufforderung die folgenden Befehle ein:

    ```text
    enable-migrations
    add-migration InitialCreate
    ```

    Der `enable-migrations` Befehl erstellt einen *Migrations* Ordner im condesouniversity-Projekt und fügt diesen Ordner in den Ordner eine *Configuration.cs* -Datei ein, die Sie zum Konfigurieren von Migrationen bearbeiten können.

    (Wenn Sie den obigen Schritt verpasst haben, der Sie zum Ändern des Daten Banknamens auffordert, finden Sie in den Migrationen die `add-migration` vorhandene Datenbank, und führen Sie den Befehl automatisch aus. Das bedeutet, dass Sie keinen Test des Migrations Codes durchführen, bevor Sie die Datenbank bereitstellen. Wenn Sie den `update-database` Befehl später ausführen, geschieht nichts, weil die Datenbank bereits vorhanden ist.)

    Öffnen Sie die Datei *contosouniversity\migrations\configuration.cs* . Wie die Initialisiererklasse, die Sie zuvor gesehen `Configuration` haben, enthält `Seed` die-Klasse eine-Methode.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

    Der Zweck der [Seed](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) -Methode besteht darin, dass Sie Testdaten einfügen oder aktualisieren können, nachdem Code First die Datenbank erstellt oder aktualisiert hat. Die-Methode wird aufgerufen, wenn die Datenbank erstellt wird, und jedes Mal, wenn das Datenbankschema nach einer Datenmodell Änderung aktualisiert wird.

### <a name="set-up-the-seed-method"></a>Einrichten der Seed-Methode

Wenn Sie die Datenbank für jede Datenmodell Änderung löschen und neu erstellen, verwenden Sie die-Methode der Initialisierer `Seed` -Klasse, um Testdaten einzufügen, da die Datenbank nach jeder Modell Änderung gelöscht wird und alle Testdaten verloren gehen. Mit Code First-Migrationen werden Testdaten nach der Daten Bank Änderung beibehalten, sodass das Einschließen von Testdaten in die [Seed](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) -Methode in der Regel nicht erforderlich ist. Sie möchten nicht, dass die `Seed` -Methode Testdaten einfügt, wenn Sie Migrationen verwenden, um die Datenbank in der Produktionsumgebung bereitzustellen, da die-Methode in der `Seed` Produktionsumgebung ausgeführt wird. In diesem Fall möchten Sie, dass `Seed` die-Methode nur die Daten, die Sie in der Produktionsumgebung benötigen, in die Datenbank einfügt. Beispielsweise kann es vorkommen, dass die Datenbank tatsächliche Abteilungsnamen in die `Department` Tabelle einschließen soll, wenn die Anwendung in der Produktionsumgebung verfügbar wird.

In diesem Tutorial verwenden Sie Migrationen für die Bereitstellung, aber Ihre `Seed` Methode fügt trotzdem Testdaten ein, um die Funktionsweise der Anwendungs Funktionalität zu vereinfachen, ohne dass viele Daten manuell eingefügt werden müssen.

1. Ersetzen Sie den Inhalt der *Configuration.cs* -Datei durch den folgenden Code, der Testdaten in die neue Datenbank lädt.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

    Die [Seed](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) -Methode nimmt das Daten Bank Kontext Objekt als Eingabeparameter an, und der Code in der-Methode verwendet dieses Objekt, um der Datenbank neue Entitäten hinzuzufügen. Für jeden Entitätstyp erstellt der Code eine Auflistung von neuen Entitäten, fügt Sie der entsprechenden [dbset](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) -Eigenschaft hinzu und speichert die Änderungen dann in der Datenbank. Es ist nicht erforderlich, die [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) -Methode nach jeder Gruppe von Entitäten aufzurufen, wie hier beschrieben, aber damit können Sie die Ursache eines Problems ermitteln, wenn eine Ausnahme auftritt, während der Code in die Datenbank schreibt.

    Einige der Anweisungen, die Daten einfügen, verwenden die [addorupdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) -Methode, um einen Upsert-Vorgang auszuführen. Da die `Seed` -Methode jedes Mal ausgeführt wird, `update-database` Wenn Sie den Befehl ausführen (in der Regel nach jeder Migration), können Sie keine Daten einfügen, da die Zeilen, die Sie hinzufügen möchten, nach der ersten Migration, mit der die Datenbank erstellt wird, bereits vorhanden sind. Der Upsert-Vorgang verhindert Fehler, die auftreten würden, wenn Sie versuchen, eine Zeile einzufügen, die bereits vorhanden ist, aber alle Änderungen an Daten ***über*** schreibt, die Sie beim Testen der Anwendung möglicherweise vorgenommen haben. Mit Testdaten in einigen Tabellen sollten Sie dies möglicherweise nicht tun: in einigen Fällen, in denen Sie Daten während des Tests ändern, möchten Sie, dass Ihre Änderungen nach Datenbankaktualisierungen verbleiben. In diesem Fall möchten Sie einen bedingten Einfügevorgang durchführen: Fügen Sie eine Zeile nur ein, wenn Sie nicht bereits vorhanden ist. Die Seed-Methode verwendet beide Ansätze.

    Der erste Parameter, der an die [addorupdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) -Methode übergeben wird, gibt die Eigenschaft an, mit der überprüft wird, ob eine Zeile bereits vorhanden ist. Für die von Ihnen bereitgestellten Test Studenten Daten kann die `LastName` -Eigenschaft für diesen Zweck verwendet werden, da jeder Nachname in der Liste eindeutig ist:

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

    Bei diesem Code wird davon ausgegangen, dass die Nachnamen eindeutig sind. Wenn Sie einen Studenten mit einem doppelten Nachnamen manuell hinzufügen, erhalten Sie die folgende Ausnahme, wenn Sie das nächste Mal eine Migration ausführen:

    **Die Sequenz enthält mehr als ein Element.**

    Informationen zum Verarbeiten von redundanten Daten wie z. b. zwei Studenten mit dem Namen "Alexander Carson" finden Sie unter [Seeding und Debugging Entity Framework (EF)-DSB](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) im Blog von Rick Anderson. Weitere Informationen zur-Methode `AddOrUpdate` finden Sie im Blog von Julie Lerman [mit der EF 4,3 addorupdate-Methode](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) .

    Der Code, der `Enrollment` Entitäten erstellt, setzt `ID` voraus, dass Sie über den `students` Wert in den Entitäten in der Auflistung verfügen, obwohl Sie diese Eigenschaft nicht im Code festgelegt haben, der die Auflistung erstellt.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=2)]

    Sie `ID` können die-Eigenschaft hier verwenden, `ID` da der Wert festgelegt wird `SaveChanges` , wenn `students` Sie für die Auflistung aufzurufen. EF Ruft den Primärschlüssel Wert automatisch ab, wenn eine Entität in die Datenbank eingefügt wird, und `ID` aktualisiert die-Eigenschaft der Entität im Arbeitsspeicher.

    Der Code, der die `Enrollment` einzelnen Entitäten `Enrollments` der Entitätenmenge `AddOrUpdate` hinzufügt, verwendet nicht die-Methode. Er überprüft, ob bereits eine Entität vorhanden ist, und fügt die Entität ein, wenn Sie nicht vorhanden ist Bei diesem Ansatz werden die Änderungen, die Sie an einer Registrierungs Qualität vornehmen, mithilfe der Benutzeroberfläche der Anwendung beibehalten. Der Code durchläuft die einzelnen Mitglieder der `Enrollment` [Liste](https://msdn.microsoft.com/library/6sh2ey19.aspx) , und wenn die Registrierung nicht in der Datenbank gefunden wird, wird die Registrierung der Datenbank hinzugefügt. Wenn Sie die Datenbank zum ersten Mal aktualisieren, ist die Datenbank leer, sodass jede Registrierung hinzugefügt wird.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

2. Erstellen Sie das Projekt.

### <a name="execute-the-first-migration"></a>Erste Migration ausführen

Wenn Sie den `add-migration` Befehl ausgeführt haben, haben Migrationen den Code generiert, mit dem die Datenbank von Grund auf neu erstellt wird. Dieser Code befindet sich auch im Ordner " *Migrationen* " in der Datei mit dem Namen  *&lt;&gt;\_Zeitstempel InitialCreate.cs*. Mit `Up` der-Methode `InitialCreate` der-Klasse werden die Datenbanktabellen erstellt, die den Datenmodell-Entitätenmengen entsprechen. Diese werden von der `Down` -Methode gelöscht.

[!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Die Migrationsfunktion ruft die Methode `Up` auf, um die Datenmodelländerungen für eine Migration zu implementieren. Wenn Sie einen Befehl eingeben, um ein Rollback für das Update auszuführen, ruft die Migrationsfunktion die Methode `Down` auf.

Dies ist die anfängliche Migration, die erstellt wurde, als Sie `add-migration InitialCreate` den Befehl eingegeben haben. Der-Parameter`InitialCreate` (in diesem Beispiel) wird als Dateiname verwendet und kann von Ihnen verwendet werden. in der Regel wählen Sie ein Wort oder einen Ausdruck aus, der zusammenfasst, was bei der Migration erfolgt. Beispielsweise können Sie eine spätere Migration &quot;adddepartmenttable&quot;benennen.

Wenn Sie die ursprüngliche Migration erstellt haben, als die Datenbank bereits vorhanden war, wird der Code für die Datenbankerstellung zwar generiert, allerdings muss er nicht ausgeführt werden, da die Datenbank bereits dem Datenmodell entspricht. Wenn Sie die App in einer anderen Umgebung bereitstellen, in der die Datenbank noch nicht vorhanden ist, wird dieser Code ausgeführt, um Ihre Datenbank zu erstellen. Daher sollte dieser zunächst getestet werden. Aus diesem Grund haben Sie den Namen der Datenbank in der Verbindungs Zeichenfolge&mdash;zuvor geändert, damit Migrationen von Grund auf neu erstellt werden können.

1. Geben Sie im Fenster **Paket-Manager-Konsole** den folgenden Befehl ein:

    `update-database`

    Der `update-database` Befehl führt die `Up` -Methode aus, um die Datenbank zu erstellen, `Seed` und führt dann die-Methode aus, um die Datenbank zu füllen. Der gleiche Prozess wird automatisch in der Produktionsumgebung ausgeführt, nachdem Sie die Anwendung bereitgestellt haben, wie im folgenden Abschnitt zu sehen ist.
2. Verwenden Sie **Server-Explorer** , um die Datenbank wie im ersten Tutorial zu überprüfen, und führen Sie die Anwendung aus, um sicherzustellen, dass alles wie zuvor funktioniert.

## <a name="deploy-to-azure"></a>Bereitstellung in Azure

Bisher wurde die Anwendung lokal in IIS Express auf dem Entwicklungs Computer ausgeführt. Um es anderen Benutzern die Verwendung über das Internet zur Verfügung zu stellen, müssen Sie Sie für einen Webhostinganbieter bereitstellen. In diesem Abschnitt des Tutorials stellen Sie die Anwendung in Azure bereit. Dieser Abschnitt ist optional. Sie können dies überspringen und mit dem folgenden Tutorial fortfahren, oder Sie können die Anweisungen in diesem Abschnitt für einen anderen Hostinganbieter Ihrer Wahl anpassen.

### <a name="use-code-first-migrations-to-deploy-the-database"></a>Verwenden von Code First Migrationen zum Bereitstellen der Datenbank

Zum Bereitstellen der-Datenbank verwenden Sie Code First-Migrationen. Wenn Sie das Veröffentlichungs Profil erstellen, das Sie zum Konfigurieren der Einstellungen für die Bereitstellung von Visual Studio verwenden, wählen Sie ein Kontrollkästchen mit der Bezeichnung **Datenbank aktualisieren**aus. Diese Einstellung bewirkt, dass der Bereitstellungs Prozess die *Web. config* -Datei der Anwendung auf dem Zielserver automatisch konfiguriert, `MigrateDatabaseToLatestVersion` sodass Code First die Initialisiererklasse verwendet.

Visual Studio führt während des Bereitstellungs Prozesses keine Schritte mit der Datenbank durch, während das Projekt auf den Zielserver kopiert wird. Wenn Sie die bereitgestellte Anwendung ausführen und zum ersten Mal nach der Bereitstellung auf die Datenbank zugreifen, überprüft Code First, ob die Datenbank mit dem Datenmodell übereinstimmt. Wenn keine Übereinstimmung vorliegt, erstellt Code First die Datenbank automatisch (sofern noch nicht vorhanden) oder aktualisiert das Datenbankschema auf die neueste Version (wenn eine Datenbank vorhanden ist, aber nicht mit dem Modell übereinstimmt). Wenn die Anwendung eine Migrations `Seed` Methode implementiert, wird die-Methode ausgeführt, nachdem die Datenbank erstellt oder das Schema aktualisiert wurde.

Die Migrations `Seed` Methode fügt Testdaten ein. Wenn Sie in einer Produktionsumgebung bereitgestellt haben, müssten Sie die `Seed` -Methode so ändern, dass nur Daten eingefügt werden, die in die Produktionsdatenbank eingefügt werden sollen. Beispielsweise können Sie in Ihrem aktuellen Datenmodell echte Kurse, aber fiktive Studenten in der Entwicklungs Datenbank haben. Sie können eine `Seed` Methode schreiben, um beide in der Entwicklung zu laden und dann die fiktiven Studenten vor der Bereitstellung in der Produktion auszukommentieren. Oder Sie können eine `Seed` Methode schreiben, um nur Kurse zu laden, und die fiktiven Studenten in der Testdatenbank manuell über die Benutzeroberfläche der Anwendung eingeben.

### <a name="get-an-azure-account"></a>Azure-Konto erhalten

Sie benötigen ein Azure-Konto. Wenn Sie noch nicht über ein Visual Studio-Abonnement verfügen, können [Sie Ihre Abonnement Vorteile](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/
)aktivieren. Andernfalls können Sie in wenigen Minuten ein kostenloses Testkonto erstellen. Weitere Informationen finden Sie unter [kostenlose Azure-Testversion](https://azure.microsoft.com/free/).

### <a name="create-a-web-site-and-a-sql-database-in-azure"></a>Erstellen einer Website und einer SQL-Datenbank in Azure

Ihre Web-App in Azure wird in einer freigegebenen Hostingumgebung ausgeführt, was bedeutet, dass Sie auf virtuellen Computern (VMS) ausgeführt wird, die gemeinsam mit anderen Azure-Clients verwendet werden. Eine freigegebene Hostingumgebung ist eine kostengünstige Möglichkeit für den Einstieg in die Cloud. Wenn der Webdatenverkehr später zunimmt, kann die Anwendung skaliert werden, um die Anforderungen zu erfüllen, indem Sie auf dedizierten VMS ausgeführt wird. Weitere Informationen zu den Preisoptionen für Azure App Service finden Sie unter [App Service Preise](https://azure.microsoft.com/pricing/details/app-service/).

Sie stellen die Datenbank in Azure SQL-Datenbank bereit. SQL-Datenbank ist ein cloudbasierter relationaler Datenbankdienst, der auf SQL Server-Technologien aufbaut. Tools und Anwendungen, die mit SQL Server arbeiten, funktionieren auch mit der SQL-Datenbank.

1. Wählen Sie im [Azure-Verwaltungsportal](https://portal.azure.com)auf der linken Registerkarte **Ressource erstellen** aus, und wählen Sie dann im **neuen** Bereich (oder im *Blatt*) die Option **alle** anzeigen aus, um alle verfügbaren Ressourcen anzuzeigen. Wählen Sie im Abschnitt **Web** des Blatts **alles** die Option Web- **App und SQL** aus. Klicken Sie abschließend auf **Erstellen**.

    ![Erstellen einer Ressource in Azure-Portal](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/create-azure-resource.png)

   Das Formular zum Erstellen einer neuen **neuen Web-App und SQL** -Ressource wird geöffnet.

2. Geben Sie eine Zeichenfolge in das Feld **App-Name** ein, um Sie als eindeutige URL für Ihre Anwendung zu verwenden. Die gesamte URL besteht aus den hier eingegebenen Optionen und der Standard Domäne Azure-App Services (. azurewebsites.net). Wenn der **App-Name** bereits verwendet wird, werden Sie vom Assistenten mit einem roten Hinweis benachrichtigt, dass *der App-Name nicht verfügbar ist* . Wenn der **App-Name** verfügbar ist, wird ein grünes Häkchensymbol angezeigt.

3. Wählen Sie im Feld **Abonnement** das Azure-Abonnement aus, in dem sich die **App Service** befinden soll.

4. Wählen Sie im Textfeld **Ressourcengruppe** eine Ressourcengruppe aus, oder erstellen Sie eine neue Ressourcengruppe. Mit dieser Einstellung wird festgelegt, in welchem Rechenzentrum die Website ausgeführt wird. Weitere Informationen zu Ressourcengruppen finden Sie unter [Ressourcengruppen](/azure/azure-resource-manager/resource-group-overview#resource-groups).

5. Erstellen Sie einen **neuen App Service Plan** , indem Sie auf den *Abschnitt App Service*klicken, **neu erstellen**und **App Service Plan** (kann denselben Namen wie App Service), **Speicherort**und Tarif ausfüllen (es gibt eine kostenlose Option).

6. Klicken Sie auf **SQL-Datenbank**und dann auf **neue Datenbank erstellen** , oder wählen Sie eine vorhandene Datenbank aus.

7. Geben Sie im Feld **Name** einen Namen für Ihre Datenbank ein.
8. Klicken Sie auf das Feld **Ziel Server** , und wählen Sie dann **neuen Server erstellen**aus. Wenn Sie zuvor einen Server erstellt haben, können Sie alternativ diesen Server aus der Liste der verfügbaren Server auswählen.
9. Wählen Sie Tarif aus, und klicken Sie auf *kostenlos*. Wenn zusätzliche Ressourcen benötigt werden, kann die Datenbank jederzeit zentral hochskaliert werden. Weitere Informationen zu den Preisen von Azure SQL finden Sie unter [Preise für Azure SQL-Datenbank](https://azure.microsoft.com/pricing/details/sql-database/managed/).
10. Ändern Sie die [Sortierung](/sql/relational-databases/collations/collation-and-unicode-support) nach Bedarf.
11. Geben Sie den Administrator **Benutzernamen** des Administrators und das SQL-Administrator **Kennwort**ein

    - Wenn Sie **neuer SQL-Datenbankserver**ausgewählt haben, definieren Sie einen neuen Namen und ein Kennwort, die Sie später beim Zugriff auf die Datenbank verwenden.
    - Wenn Sie einen Server ausgewählt haben, den Sie zuvor erstellt haben, geben Sie die Anmelde Informationen für diesen Server ein.

12. Die telemetrieauflistung kann mithilfe Application Insights für App Service aktiviert werden. Bei geringem Konfigurationsaufwand sammelt Application Insights wertvolle Informationen zu Ereignissen, Ausnahmen, Abhängigkeiten, Anforderungen und Ablauf Verfolgungen. Weitere Informationen zu Application Insights finden Sie unter [Azure Monitor](https://azure.microsoft.com/services/monitor/).
13. Klicken Sie unten auf **Erstellen** , um anzugeben, dass Sie fertig sind.

    Der Verwaltungsportal wird zur Seite Dashboard zurückgegeben, und im Bereich **Benachrichtigungen** oben auf der Seite wird angezeigt, dass die Website erstellt wird. Nach einer Weile (in der Regel weniger als eine Minute) gibt es eine Benachrichtigung, dass die Bereitstellung erfolgreich war. In der Navigationsleiste auf der linken Seite wird der neue APP Service im Abschnitt **App Services** angezeigt, und die neue SQL-Datenbank wird im Abschnitt **SQL-Datenbanken** angezeigt.

### <a name="deploy-the-app-to-azure"></a>Bereitstellen der App in Azure

1. Klicken Sie in Visual Studio mit der rechten Maustaste auf das Projekt in **Projektmappen-Explorer** , und wählen Sie im Kontextmenü **veröffentlichen** aus.

2. Wählen Sie auf der Seite **Veröffentlichungsziel auswählen** die Option **App Service** aus, und wählen **Sie dann vorhandene**und dann **veröffentlichen**aus.

    ![Seite "Veröffentlichungsziel auswählen"](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/publish-select-existing-azure-app-service.png)

3. Wenn Sie Ihr Azure-Abonnement nicht bereits in Visual Studio hinzugefügt haben, führen Sie die Schritte auf dem Bildschirm aus. Mit diesen Schritten kann Visual Studio eine Verbindung mit Ihrem Azure-Abonnement herstellen, sodass die Liste der **App Services** Ihre Website enthält.

4. Wählen Sie auf der Seite **App Service** das **Abonnement** aus, dem Sie die APP Service hinzugefügt haben. Wählen Sie unter **Ansicht**die Option **Ressourcengruppe**aus. Erweitern Sie die Ressourcengruppe, der Sie die APP Service hinzugefügt haben, und wählen Sie dann den App Service aus. Wählen Sie **OK** aus, um die APP zu veröffentlichen.

5. Das Fenster **Ausgabe** zeigt, welche Bereitstellungs Aktionen ausgeführt wurden, und meldet einen erfolgreichen Abschluss der Bereitstellung.

6. Nach erfolgreicher Bereitstellung wird der Standardbrowser automatisch mit der URL der bereitgestellten Website geöffnet.

    ![Students_index_page_with_paging](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/cloud-app-browser.png)

    Ihre APP wird jetzt in der Cloud ausgeführt.

An diesem Punkt wurde die *schoolContext* -Datenbank in der Azure SQL-Datenbank erstellt, weil Sie die Option **Code First-Migrationen ausführen ausgewählt haben (wird beim APP-Start ausgeführt)** . Die Datei " *Web. config* " auf der bereitgestellten Website wurde geändert, sodass der [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) -Initialisierer beim ersten Lesen oder Schreiben von Daten in der Datenbank ausgeführt wird (was geschah, als Sie die Registerkarte " **Students** " ausgewählt haben). ):

![Auszug der Datei "Web. config"](https://asp.net/media/4367421/mig.png)

Beim Bereitstellungs Prozess wurde außerdem eine neue Verbindungs Zeichenfolge *(schoolContext\_databasepublish*) für Code First-Migrationen erstellt, die zum Aktualisieren des Datenbankschemas und zum Seeding der Datenbank verwendet werden soll.

![Verbindungs Zeichenfolge in der Datei "Web. config"](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image26.png)

Die bereitgestellte Version der Datei "Web. config" finden Sie auf Ihrem eigenen Computer unter *condesouniversity\obj\release\package\packagetmp\web.config*. Sie können auf die bereitgestellte *Web. config* -Datei selbst über FTP zugreifen. Anweisungen finden [Sie unter ASP.net Web Deployment using Visual Studio: Bereitstellen eines Code](xref:web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update)Updates. Befolgen Sie die Anweisungen, die mit "So verwenden Sie ein FTP-Tool" beginnen: die FTP-URL, der Benutzername und das Kennwort. "

> [!NOTE]
> Die Web-App implementiert keine Sicherheit, sodass jeder, der die URL findet, die Daten ändern kann. Anweisungen zum Sichern der Website finden Sie unter Bereitstellen [einer Secure ASP.NET MVC-App mit Mitgliedschaft, OAuth und SQL-Datenbank in Azure](/aspnet/core/security/authorization/secure-data). Sie können verhindern, dass andere Personen die Website verwenden, indem Sie den Dienst mithilfe des Azure-Verwaltungsportal oder **Server-Explorer** in Visual Studio beenden.

![Menü Element "App Service" Abbrechen](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/server-explorer-stop-app-service.png)

## <a name="advanced-migrations-scenarios"></a>Erweiterte Migrations Szenarios

Wenn Sie eine Datenbank bereitstellen, indem Sie wie in diesem Tutorial gezeigt automatisch Migrationen ausführen und die Bereitstellung auf einer Website durchführen, die auf mehreren Servern ausgeführt wird, können Sie mehrere Server abrufen, die versuchen, Migrationen gleichzeitig auszuführen. Migrationen sind atomarisch. Wenn also zwei Server versuchen, dieselbe Migration auszuführen, wird eine solche Migration erfolgreich ausgeführt, die andere schlägt fehl (vorausgesetzt, die Vorgänge können nicht zweimal ausgeführt werden). Wenn Sie diese Probleme vermeiden möchten, können Sie Migrationen manuell aufzurufen und ihren eigenen Code so einrichten, dass er nur einmal ausgeführt wird. Weitere Informationen finden Sie unter [ausführen und Durchführen von Skript Migrationen aus Code](http://romiller.com/2012/02/09/running-scripting-migrations-from-code/) im Blog von Rowan Miller und [Migrieren der exe](/ef/ef6/modeling/code-first/migrations/migrate-exe) -Datei (zum Ausführen von Migrationen über die Befehlszeile).

Weitere Informationen zu anderen Migrationsszenarien finden Sie unter [Migrationen von screencastreihen](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx).

## <a name="update-specific-migration"></a>Bestimmte Migration aktualisieren

`update-database -target MigrationName`

Der `update-database -target MigrationName` Befehl führt die gezielte Migration aus.

## <a name="ignore-migration-changes-to-database"></a>Migrations Änderungen in Datenbank ignorieren

`Add-migration MigrationName -ignoreChanges`

`ignoreChanges`erstellt eine leere Migration mit dem aktuellen Modell als Momentaufnahme.

## <a name="code-first-initializers"></a>Code First-Initialisierer

Im Abschnitt Bereitstellung sehen Sie, dass der [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) -Initialisierer verwendet wird. Code First bietet auch andere Initialisierer, einschließlich " [kreatedatabaseifnotexists](https://msdn.microsoft.com/library/gg679221(v=vs.103).aspx) " (Standardeinstellung), " [dropkreatedatabaseifmodelchanges](https://msdn.microsoft.com/library/gg679604(v=VS.103).aspx) " (die Sie zuvor verwendet haben) und " [dropkreatedatabasealways](https://msdn.microsoft.com/library/gg679506(v=VS.103).aspx)". Der `DropCreateAlways` Initialisierer kann beim Einrichten von Bedingungen für Komponententests hilfreich sein. Sie können auch eigene Initialisierer schreiben, und Sie können einen Initialisierer explizit aufzurufen, wenn Sie nicht warten möchten, bis die Anwendung aus der Datenbank liest oder in diese schreibt.

Weitere Informationen zu Initialisierern finden Sie Untergrund Legendes zu [datenbankinitialisierern in Entity Framework Code First](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm) und Kapitel 6 [der Buch Programmierung Entity Framework: Code First](http://shop.oreilly.com/product/0636920022220.do) von Julie Lerman und Rowan Miller.

## <a name="get-the-code"></a>Abrufen des Codes

[Herunterladen des abgeschlossenen Projekts](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Links zu anderen Entity Framework Ressourcen finden Sie unter [ASP.NET Data Access-Empfohlene Ressourcen](xref:whitepapers/aspnet-data-access-content-map).

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial:

> [!div class="checklist"]
> * Aktivierte Code First Migrationen
> * Bereitstellung der app in Azure (optional)

Fahren Sie mit dem nächsten Artikel fort, um zu erfahren, wie Sie ein komplexeres Datenmodell für eine ASP.NET MVC-Anwendung erstellen.
> [!div class="nextstepaction"]
> [Erstellen eines komplexeren Datenmodells](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
