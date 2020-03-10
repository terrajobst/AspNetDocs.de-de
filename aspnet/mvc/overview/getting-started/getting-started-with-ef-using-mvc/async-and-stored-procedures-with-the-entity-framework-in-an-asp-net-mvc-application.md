---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Tutorial: Verwenden von Async-und gespeicherten Prozeduren mit EF in einer ASP.NET MVC-App'
description: In diesem Tutorial erfahren Sie, wie Sie das asynchrone Programmiermodell implementieren und erfahren, wie Sie gespeicherte Prozeduren verwenden.
author: tdykstra
ms.author: riande
ms.date: 01/18/2019
ms.topic: tutorial
ms.assetid: 27d110fc-d1b7-4628-a763-26f1e6087549
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 5612f2f25d06feb904a205505ed8f048d2263266
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78471387"
---
# <a name="tutorial-use-async-and-stored-procedures-with-ef-in-an-aspnet-mvc-app"></a>Tutorial: Verwenden von Async-und gespeicherten Prozeduren mit EF in einer ASP.NET MVC-App

In den vorherigen Tutorials haben Sie gelernt, wie Sie Daten mit dem synchronen Programmiermodell lesen und aktualisieren. In diesem Tutorial erfahren Sie, wie Sie das asynchrone Programmiermodell implementieren. Mithilfe von asynchronem Code kann eine Anwendung besser funktionieren, da Server Ressourcen besser genutzt werden.

In diesem Tutorial erfahren Sie auch, wie Sie gespeicherte Prozeduren für Einfüge-, Aktualisierungs-und Löschvorgänge für eine Entität verwenden.

Schließlich stellen Sie die Anwendung zusammen mit allen Daten Bank Änderungen, die Sie seit der erstmaligen Bereitstellung implementiert haben, in Azure erneut bereit.

In den folgenden Abbildungen werden die Seiten dargestellt, mit denen Sie arbeiten werden.

![Abteilungs Seite](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Abteilung erstellen](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

In diesem Tutorial führen Sie Folgendes durch:

> [!div class="checklist"]
> * Weitere Informationen zu asynchronem Code
> * Erstellen eines Abteilungs Controllers
> * Gespeicherte Prozeduren verwenden
> * Bereitstellung in Azure

## <a name="prerequisites"></a>Voraussetzungen

* [Aktualisieren von relevanten Daten](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="why-use-asynchronous-code"></a>Gründe für die Verwendung von asynchronem Code

Der Webserver verfügt nur über eine begrenzte Anzahl von Threads. Daher werden bei hoher Auslastung möglicherweise alle verfügbaren Threads gleichzeitig verwendet. Wenn dies der Fall ist, kann der Server keine neuen Anforderungen verarbeiten, bis die Threads wieder freigegeben werden. Wenn synchroner Code verwendet wird, kann es sein, dass zwar viele Threads belegt sind, diese aber keine Vorgänge ausführen, da sie auf den Abschluss der E/A-Vorgänge warten. Wenn asynchroner Code verwendet wird, werden Threads für den Server freigegeben, wenn diese nur auf den Abschluss der E/A-Vorgänge warten, damit andere Anforderungen verarbeitet werden können. Folglich ermöglicht der asynchrone Code die effizientere Verwendung von Server Ressourcen, und der Server ist für die Verarbeitung von mehr Datenverkehr ohne Verzögerungen aktiviert.

In früheren Versionen von .net war das Schreiben und Testen von asynchronem Code komplex, fehleranfällig und schwer zu debuggen. In .NET 4,5 ist das Schreiben, testen und Debuggen von asynchronem Code sehr viel einfacher, wenn Sie in der Regel asynchronen Code schreiben sollten, es sei denn, Sie haben einen Grund für nicht. Der asynchrone Code führt zu einem geringen Aufwand, aber bei Problemen mit geringem Datenverkehr ist die Leistungseinbußen unerheblich, und bei großen Datenverkehrs Fällen ist die potenzielle Leistungsverbesserung beträchtlich.

Weitere Informationen zur asynchronen Programmierung finden Sie unter [Verwenden der async-Unterstützung von .NET 4.5, um blockierende Aufrufe zu vermeiden](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices.md#async).

## <a name="create-department-controller"></a>Abteilungs Controller erstellen

Erstellen Sie einen Abteilungs Controller auf die gleiche Weise wie die früheren Controller, und aktivieren Sie das Kontrollkästchen **Async Controller-Aktionen verwenden** .

Die folgenden Highlights zeigen, was dem synchronen Code für die `Index`-Methode hinzugefügt wurde, um Sie asynchron zu machen:

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=1,4)]

Es wurden vier Änderungen angewendet, um die asynchrone Ausführung der Entity Framework Datenbankabfrage zu ermöglichen:

- Die-Methode wird mit dem `async`-Schlüsselwort gekennzeichnet, das den Compiler anweist, Rückrufe für Teile des Methoden Texts zu generieren und automatisch das `Task<ActionResult>` Objekt zu erstellen, das zurückgegeben wird.
- Der Rückgabetyp wurde von `ActionResult` in `Task<ActionResult>`geändert. Der `Task<T>`-Typ stellt laufende Arbeit mit einem Ergebnis vom Typ `T`dar.
- Das `await`-Schlüsselwort wurde auf den Webdienst aufzurufen. Wenn der Compiler dieses Schlüsselwort sieht, wird die Methode im Hintergrund in zwei Teile aufgeteilt. Der erste Teil endet mit dem Vorgang, der asynchron gestartet wird. Der zweite Teil wird in eine Rückruf Methode eingefügt, die aufgerufen wird, wenn der Vorgang abgeschlossen wird.
- Die asynchrone Version der `ToList` Erweiterungsmethode wurde aufgerufen.

Warum wird die `departments.ToList`-Anweisung geändert, aber nicht die `departments = db.Departments`-Anweisung? Der Grund hierfür ist, dass nur Anweisungen, die dazu führen, dass Abfragen oder Befehle an die Datenbank gesendet werden, asynchron ausgeführt werden. Die `departments = db.Departments`-Anweisung richtet eine Abfrage ein, die Abfrage wird jedoch erst ausgeführt, wenn die `ToList`-Methode aufgerufen wird. Daher wird nur die `ToList`-Methode asynchron ausgeführt.

In den Methoden `Details` und `HttpGet` `Edit` und `Delete` ist die `Find`-Methode die Methode, die bewirkt, dass eine Abfrage an die Datenbank gesendet wird. Dies ist also die Methode, die asynchron ausgeführt wird:

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs?highlight=7)]

In den Methoden `Create`, `HttpPost Edit`und `DeleteConfirmed` ist es der `SaveChanges` Methodenaufrufe, der bewirkt, dass ein Befehl ausgeführt wird, und keine Anweisungen wie `db.Departments.Add(department)`, die nur die Änderung von Entitäten im Arbeitsspeicher bewirken.

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=6)]

Öffnen Sie *views\department\index.cshtml*, und ersetzen Sie den Vorlagen Code durch den folgenden Code:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=3,5,20-22,36-38)]

Dieser Code ändert den Titel von Index in Abteilungen, verschiebt den Administrator Namen nach rechts und gibt den vollständigen Namen des Administrators an.

Ändern Sie in den Ansichten erstellen, löschen, Details und bearbeiten die Beschriftung für das Feld `InstructorID` auf die gleiche Weise, wie Sie in den Kurs Ansichten das Feld Abteilungs Name in "Abteilung" geändert haben.

Verwenden Sie in den Ansichten erstellen und bearbeiten den folgenden Code:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml)]

Verwenden Sie in den Ansichten "Löschen" und "Details" den folgenden Code:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

Führen Sie die Anwendung aus, und klicken Sie auf die Registerkarte **Abteilungen** .

Alles funktioniert genauso wie in den anderen Controllern, aber in diesem Controller werden alle SQL-Abfragen asynchron ausgeführt.

Einige Dinge, die Sie beachten sollten, wenn Sie die asynchrone Programmierung mit dem Entity Framework verwenden:

- Der Async-Code ist nicht Thread sicher. Anders ausgedrückt: versuchen Sie nicht, mehrere Vorgänge parallel mithilfe derselben Kontext Instanz auszuführen.
- Wenn Sie von den Leistungsvorteilen des asynchronen Codes profitieren möchten, vergewissern Sie sich, dass auch alle Bibliothekspakete, die Sie verwenden (z.B. zum Paging) asynchronen Code verwenden, wenn sie Entity Framework Core-Methoden aufrufen, die Abfragen an die Datenbank senden.

## <a name="use-stored-procedures"></a>Gespeicherte Prozeduren verwenden

Einige Entwickler und DBAs bevorzugen die Verwendung gespeicherter Prozeduren für den Datenbankzugriff. In früheren Versionen von Entity Framework Sie Daten mithilfe einer gespeicherten Prozedur abrufen, indem Sie eine unformatierte [SQL-Abfrage ausführen](advanced-entity-framework-scenarios-for-an-mvc-web-application.md). Sie können EF jedoch nicht anweisen, gespeicherte Prozeduren für Aktualisierungs Vorgänge zu verwenden. In EF 6 ist es einfach, Code First für die Verwendung gespeicherter Prozeduren zu konfigurieren.

1. Fügen Sie in " *dal\schoolcontext.cs*" den hervorgehobenen Code der `OnModelCreating`-Methode hinzu.

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=9)]

    Dieser Code weist Entity Framework an, gespeicherte Prozeduren für Einfüge-, Aktualisierungs-und Löschvorgänge für die `Department` Entität zu verwenden.
2. Geben Sie in der Konsole Package Manage den folgenden Befehl ein:

    `add-migration DepartmentSP`

    Öffnen Sie *Migrationen\\&lt;Zeitstempel&gt;\_DepartmentSP.cs* , um den Code in der `Up`-Methode anzuzeigen, mit der gespeicherte Prozeduren zum Einfügen, aktualisieren und löschen erstellt werden:

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs?highlight=3-4,26-27,42-43)]
3. Geben Sie in der Konsole Package Manage den folgenden Befehl ein:

     `update-database`
4. Führen Sie die Anwendung im Debugmodus aus, klicken Sie auf die Registerkarte **Abteilungen** und dann auf **neu erstellen**.
5. Geben Sie Daten für eine neue Abteilung ein, und klicken Sie dann auf **Erstellen**.

6. Überprüfen Sie in Visual Studio die Protokolle im **Ausgabe** Fenster, um zu sehen, dass eine gespeicherte Prozedur zum Einfügen der neuen Abteilungs Zeile verwendet wurde.

     ![SP der Abteilung einfügen](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Code First erstellt Standardnamen für gespeicherte Prozeduren. Wenn Sie eine vorhandene Datenbank verwenden, müssen Sie ggf. die Namen der gespeicherten Prozeduren anpassen, damit Sie bereits in der Datenbank definierte gespeicherte Prozeduren verwenden können. Weitere Informationen hierzu finden Sie unter [Entity Framework Code First Einfügen/Aktualisieren/Löschen gespeicherter Prozeduren](https://msdn.microsoft.com/data/dn468673).

Wenn Sie anpassen möchten, welche generierten gespeicherten Prozeduren Sie ausführen, können Sie den Gerüst Code für die Migrationen `Up` Methode bearbeiten, mit der die gespeicherte Prozedur erstellt wird. Auf diese Weise werden Ihre Änderungen bei jedem Ausführen der Migration reflektiert und auf die Produktionsdatenbank angewendet, wenn Migrationen nach der Bereitstellung automatisch in der Produktionsumgebung ausgeführt werden.

Wenn Sie eine vorhandene gespeicherte Prozedur ändern möchten, die in einer vorherigen Migration erstellt wurde, können Sie mit dem Befehl Add-Migration eine leere Migration generieren und dann manuell Code schreiben, der die [alterstoredprocedure](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.alterstoredprocedure.aspx) -Methode aufruft.

## <a name="deploy-to-azure"></a>Bereitstellung in Azure

In diesem Abschnitt ist es erforderlich, dass Sie den Abschnitt optionale Bereitstellung **der APP für Azure** im Tutorial [Migrationen und Bereitstellung](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md) dieser Reihe abgeschlossen haben. Wenn Migrations Fehler aufgetreten sind, die Sie durch Löschen der Datenbank im lokalen Projekt aufgelöst haben, überspringen Sie diesen Abschnitt.

1. Klicken Sie im **Projektmappen-Explorer** von Visual Studio mit der rechten Maustaste auf das Projekt, und wählen Sie im Kontextmenü **Veröffentlichen** aus.
2. Klicken Sie auf **Veröffentlichen**.

    Visual Studio stellt die Anwendung in Azure bereit, und die Anwendung wird in Ihrem Standardbrowser geöffnet, der in Azure ausgeführt wird.
3. Testen Sie die Anwendung, um zu überprüfen, ob Sie funktioniert.

    Wenn Sie zum ersten Mal eine Seite ausführen, die auf die Datenbank zugreift, führt die Entity Framework alle Migrationen `Up` Methoden aus, die erforderlich sind, um die Datenbank mit dem aktuellen Datenmodell auf den neuesten Stand zu bringen. Sie können jetzt alle Webseiten verwenden, die Sie seit der letzten Bereitstellung hinzugefügt haben, einschließlich der Abteilungs Seiten, die Sie in diesem Tutorial hinzugefügt haben.

## <a name="get-the-code"></a>Abrufen des Codes

[Herunterladen des abgeschlossenen Projekts](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Links zu anderen Entity Framework Ressourcen finden Sie unter [ASP.NET Data Access-Empfohlene Ressourcen](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial führen Sie Folgendes durch:

> [!div class="checklist"]
> * Erlernen von asynchronem Code
> * Erstellen eines Abteilungs Controllers
> * Verwendete gespeicherte Prozeduren
> * Bereitstellung in Azure

Im nächsten Artikel erfahren Sie, wie Sie Konflikte behandeln, wenn mehrere Benutzer gleichzeitig dieselbe Entität aktualisieren.
> [!div class="nextstepaction"]
> [Behandeln von Parallelität](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
