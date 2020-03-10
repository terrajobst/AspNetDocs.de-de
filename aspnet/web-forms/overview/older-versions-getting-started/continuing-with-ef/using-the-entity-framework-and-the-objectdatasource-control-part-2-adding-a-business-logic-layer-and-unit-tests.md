---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
title: 'Mit dem Entity Framework 4,0 und dem ObjectDataSource-Steuerelement, Teil 2: Hinzufügen einer Geschäftslogik Schicht und Komponenten Tests | Microsoft-Dokumentation'
author: tdykstra
description: Diese tutorialreihe basiert auf der Webanwendung der Website von "Web", die in der tutorialreihe für die ersten Schritte mit Entity Framework 4,0 erstellt wurde. I...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: efb0e677-10b8-48dc-93d3-9ba3902dd807
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
msc.type: authoredcontent
ms.openlocfilehash: 24344cc33d7c26d7c408db26c0530ef2c708a7d3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78439953"
---
# <a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests"></a>Mit dem Entity Framework 4,0 und dem ObjectDataSource-Steuerelement, Teil 2: Hinzufügen einer Geschäftslogik Schicht und Komponenten Tests

von [Tom Dykstra](https://github.com/tdykstra)

> Diese tutorialreihe basiert auf der Webanwendung der Website von "Web", die in der tutorialreihe für die ersten Schritte [mit Entity Framework 4,0](https://asp.net/entity-framework/tutorials#Getting%20Started) erstellt wurde. Wenn Sie die vorherigen Tutorials nicht durchgearbeitet haben, können Sie die Anwendung, die Sie erstellt haben, als Ausgangspunkt für dieses Tutorial [herunterladen](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) . Sie können auch [die Anwendung herunterladen](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) , die von der kompletten tutorialreihe erstellt wird. Wenn Sie Fragen zu den Tutorials haben, können Sie Sie im ASP.net- [Entity Framework Forum](https://forums.asp.net/1227.aspx)Posten.

Im vorherigen Tutorial haben Sie eine n-Tier-Webanwendung mit dem-Entity Framework und dem `ObjectDataSource`-Steuerelement erstellt. In diesem Tutorial wird gezeigt, wie Geschäftslogik hinzugefügt wird, während die Geschäftslogik Schicht (Business Logic Layer, BLL) und die Datenzugriffs Schicht (Data-Access Layer, DAL) getrennt bleiben. Außerdem wird gezeigt, wie automatisierte Komponententests für die BLL erstellt werden.

In diesem Tutorial führen Sie die folgenden Aufgaben aus:

- Erstellen Sie eine Repository-Schnittstelle, die die benötigten Datenzugriffs Methoden deklariert.
- Implementieren Sie die Repository-Schnittstelle in der Repository-Klasse.
- Erstellen Sie eine Geschäftslogik Klasse, die die Repository-Klasse aufruft, um Datenzugriffs Funktionen auszuführen.
- Verbinden Sie das `ObjectDataSource`-Steuerelement mit der Geschäftslogik Klasse anstelle der Repository-Klasse.
- Erstellen Sie ein Komponenten Testprojekt und eine Repository-Klasse, die in-Memory-Auflistungen für Ihren Datenspeicher verwendet.
- Erstellen Sie einen Komponenten Test für Geschäftslogik, den Sie der Geschäftslogik Klasse hinzufügen möchten, und führen Sie dann den Test aus, und sehen Sie, dass er fehlschlägt.
- Implementieren Sie die Geschäftslogik in der Geschäftslogik Klasse, führen Sie den Komponenten Test erneut aus, und überprüfen Sie den Komponenten Test.

Sie arbeiten mit den Seiten " *Departments. aspx* " und " *departmentsadd. aspx* ", die Sie im vorherigen Tutorial erstellt haben.

## <a name="creating-a-repository-interface"></a>Erstellen einer Repository-Schnittstelle

Zunächst erstellen Sie die Repository-Schnittstelle.

[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image1.png)

Erstellen Sie im Ordner *dal* eine neue Klassendatei, nennen Sie Sie *ISchoolRepository.cs*, und ersetzen Sie den vorhandenen Code durch den folgenden Code:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample1.cs)]

Die-Schnittstelle definiert eine Methode für jede der CRUD (Create, Read, Update, DELETE)-Methoden, die Sie in der Repository-Klasse erstellt haben.

Geben Sie in der `SchoolRepository`-Klasse in *SchoolRepository.cs*an, dass diese Klasse die `ISchoolRepository`-Schnittstelle implementiert:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample2.cs)]

## <a name="creating-a-business-logic-class"></a>Erstellen einer Geschäftslogik Klasse

Als Nächstes erstellen Sie die Geschäftslogik Klasse. Dies geschieht, damit Sie Geschäftslogik hinzufügen können, die vom `ObjectDataSource`-Steuerelement ausgeführt wird, obwohl dies noch nicht der Fall ist. Derzeit führt die neue Geschäftslogik Klasse nur die gleichen CRUD-Vorgänge aus, die das Repository ausführt.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image3.png)

Erstellen Sie einen neuen Ordner, und benennen Sie ihn mit *BLL*. (In einer realen Anwendung würde die Geschäftslogik Schicht in der Regel als Klassenbibliothek implementiert werden – ein separates Projekt – aber um dieses Tutorial einfach zu halten, werden BLL-Klassen in einem Projektordner aufbewahrt.)

Erstellen Sie im Ordner *BLL* eine neue Klassendatei, nennen Sie Sie *SchoolBL.cs*, und ersetzen Sie den vorhandenen Code durch den folgenden Code:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample3.cs)]

Mit diesem Code werden die gleichen CRUD-Methoden erstellt, die Sie zuvor in der Repository-Klasse gesehen haben. anstatt direkt auf die Entity Framework Methoden zuzugreifen, werden die Methoden der Repository-Klasse aufgerufen.

Die Klassen Variable, die einen Verweis auf die Repository-Klasse enthält, wird als Schnittstellentyp definiert, und der Code, der die Repository-Klasse instanziiert, ist in zwei Konstruktoren enthalten. Der Parameter lose Konstruktor wird vom `ObjectDataSource`-Steuerelement verwendet. Es wird eine Instanz der `SchoolRepository`-Klasse erstellt, die Sie zuvor erstellt haben. Der andere Konstruktor ermöglicht den Code, der die Geschäftslogik Klasse instanziiert, an jedes Objekt, das die Repository-Schnittstelle implementiert.

Die CRUD-Methoden, mit denen die Repository-Klasse und die beiden Konstruktoren aufgerufen werden, ermöglichen die Verwendung der Geschäftslogik Klasse mit jedem beliebigen Back-End-Datenspeicher, den Sie auswählen. Die Geschäftslogik Klasse muss nicht wissen, wie die von ihr aufrufende Klasse die Daten persistent speichert. (Dies wird häufig als *Persistenzignoranz*bezeichnet.) Dies vereinfacht Komponententests, da Sie die Business-Logic-Klasse mit einer Repository-Implementierung verbinden können, die etwas so einfaches wie in-Memory-`List` Sammlungen zum Speichern von Daten verwendet.

> [!NOTE]
> Technisch gesehen sind die Entitäts Objekte nach wie vor keine Persistenz, da Sie von Klassen instanziiert werden, die von der `EntityObject` Klasse des Entity Framework erben. Um die Unkenntnis der Persistenz zu gewährleisten, können Sie *einfache alte CLR-Objekte*oder *POCOS*anstelle von Objekten verwenden, die von der `EntityObject`-Klasse erben. Die Verwendung von POCOS geht über den Rahmen dieses Tutorials hinaus. Weitere Informationen finden Sie auf der MSDN-Website unter [Testability und Entity Framework 4,0](https://msdn.microsoft.com/library/ff714955.aspx) .)

Nun können Sie die `ObjectDataSource`-Steuerelemente mit der Geschäftslogik Klasse verbinden, anstatt Sie mit dem Repository zu verbinden und zu überprüfen, ob alles wie zuvor funktioniert.

Ändern Sie in " *Departments. aspx* " und " *departmentsadd. aspx*" jedes Vorkommen von `TypeName="ContosoUniversity.DAL.SchoolRepository"` in "`TypeName="ContosoUniversity.BLL.SchoolBL`". (Es sind insgesamt vier Instanzen vorhanden.)

Führen Sie die Seiten " *Departments. aspx* " und " *departmentsadd. aspx* " aus, um zu überprüfen, ob Sie weiterhin wie zuvor funktionieren.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image5.png)

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image7.png)

## <a name="creating-a-unit-test-project-and-repository-implementation"></a>Erstellen eines Komponenten Test Projekts und einer Repository-Implementierung

Fügen Sie der Projekt Mappe mithilfe der Vorlage **Test Projekt** ein neues Projekt hinzu, und benennen Sie es `ContosoUniversity.Tests`.

Fügen Sie im Testprojekt einen Verweis auf `System.Data.Entity` hinzu, und fügen Sie dem Projekt `ContosoUniversity` einen Projekt Verweis hinzu.

Sie können jetzt die Repository-Klasse erstellen, die Sie mit Komponententests verwenden werden. Der Datenspeicher für dieses Repository ist innerhalb der-Klasse.

[![Image12](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image9.png)

Erstellen Sie im Testprojekt eine neue Klassendatei, nennen Sie Sie *MockSchoolRepository.cs*, und ersetzen Sie den vorhandenen Code durch den folgenden Code:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample4.cs)]

Diese Repository-Klasse verfügt über die gleichen CRUD-Methoden wie die, die direkt auf die Entity Framework zugreift, aber Sie funktioniert mit `List` Auflistungen im Arbeitsspeicher und nicht mit einer-Datenbank. Dies erleichtert es einer Testklasse, Komponententests für die Geschäftslogik Klasse einzurichten und zu validieren.

## <a name="creating-unit-tests"></a>Erstellen von Komponenten Tests

Die **Test** Projektvorlage hat eine Stubkomponenten Test-Klasse für Sie erstellt, und die nächste Aufgabe besteht darin, diese Klasse zu ändern, indem Sie Komponenten Testmethoden für die Geschäftslogik hinzufügt, die Sie der Geschäftslogik Klasse hinzufügen möchten.

[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image11.png)

Bei der IT-Abteilung von "IT" können alle einzelnen Dozenten nur Administrator einer einzelnen Abteilung sein, und Sie müssen Geschäftslogik hinzufügen, um diese Regel zu erzwingen. Sie beginnen mit dem Hinzufügen von Tests und Ausführen der Tests, um zu sehen, dass Sie fehlschlagen. Fügen Sie dann den Code hinzu, und führen Sie die Tests erneut aus, und erwarten Sie, dass Sie erfolgreich sind.

Öffnen Sie die Datei *UnitTest1.cs* , und fügen Sie `using`-Anweisungen für die Geschäftslogik und die Datenzugriffsebenen hinzu, die Sie im Projekt contosouniversity erstellt haben:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample5.cs)]

Ersetzen Sie die `TestMethod1`-Methode durch die folgenden Methoden:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample6.cs)]

Die `CreateSchoolBL`-Methode erstellt eine Instanz der Repository-Klasse, die Sie für das Komponenten Testprojekt erstellt haben, die dann an eine neue Instanz der Business-Logic-Klasse weitergeleitet wird. Die-Methode verwendet dann die Business-Logic-Klasse, um drei Abteilungen einzufügen, die Sie in Testmethoden verwenden können.

Die Testmethoden überprüfen, ob die Geschäftslogik Klasse eine Ausnahme auslöst, wenn jemand versucht, eine neue Abteilung mit demselben Administrator wie eine vorhandene Abteilung einzufügen, oder wenn jemand versucht, den Administrator einer Abteilung zu aktualisieren, indem er auf die ID einer Person festgelegt wird. der, der bereits Administrator einer anderen Abteilung ist.

Sie haben noch keine Ausnahme Klasse erstellt, daher wird dieser Code nicht kompiliert. Um die Kompilierung zu kompilieren, klicken Sie mit der rechten Maustaste auf `DuplicateAdministratorException`, und wählen Sie **generieren**und dann **Klasse**aus.

[![Image14](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image13.png)

Dadurch wird im Testprojekt eine Klasse erstellt, die Sie nach dem Erstellen der Ausnahme Klasse im Hauptprojekt löschen können. und die Geschäftslogik implementiert.

Führen Sie das Testprojekt aus. Erwartungsgemäß schlägt der Test fehl.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image15.png)

## <a name="adding-business-logic-to-make-a-test-pass"></a>Hinzufügen von Geschäftslogik zum Durchführen eines Test Durchlaufs

Als nächstes implementieren Sie die Geschäftslogik, die es unmöglich macht, als Administrator einer Abteilung festzulegen, die bereits Administrator einer anderen Abteilung ist. Sie lösen eine Ausnahme von der Geschäftslogik Schicht aus und fangen Sie dann in der Darstellungs Schicht ab, wenn ein Benutzer eine Abteilung bearbeitet und auf **Aktualisieren** klickt, nachdem jemand ausgewählt wurde, der bereits Administrator ist. (Sie können Dozenten auch aus der Dropdown Liste entfernen, die bereits Administratoren sind, bevor Sie die Seite Renderern, aber der Zweck besteht darin, mit der Geschäftslogik Schicht zu arbeiten.)

Beginnen Sie mit dem Erstellen der Ausnahme Klasse, die Sie auslösen, wenn ein Benutzer versucht, einen Dozenten zu einem Administrator von mehreren Abteilungen zu machen. Erstellen Sie im Hauptprojekt eine neue Klassendatei im Ordner *BLL* , nennen Sie Sie *DuplicateAdministratorException.cs*, und ersetzen Sie den vorhandenen Code durch den folgenden Code:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample7.cs)]

Löschen Sie jetzt die temporäre Datei *DuplicateAdministratorException.cs* , die Sie zuvor im Testprojekt erstellt haben, damit Sie kompiliert werden kann.

Öffnen Sie im Hauptprojekt die Datei *SchoolBL.cs* , und fügen Sie die folgende Methode hinzu, die die Validierungs Logik enthält. (Der Code verweist auf eine Methode, die Sie später erstellen.)

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample8.cs)]

Diese Methode wird aufgerufen, wenn Sie `Department` Entitäten einfügen oder aktualisieren, um zu überprüfen, ob eine andere Abteilung bereits denselben Administrator hat.

Der Code Ruft eine Methode auf, um die Datenbank nach einer `Department` Entität zu durchsuchen, die denselben `Administrator`-Eigenschafts Wert aufweist wie die Entität, die eingefügt oder aktualisiert wird. Wenn ein solcher gefunden wird, löst der Code eine Ausnahme aus. Wenn die Entität, die eingefügt oder aktualisiert wird, keinen `Administrator` Wert hat, ist keine Überprüfungs Überprüfung erforderlich, und es wird keine Ausnahme ausgelöst, wenn die Methode während einer Aktualisierung aufgerufen wird und die gefundene `Department` Entität mit der `Department` Entität übereinstimmt, die aktualisiert wird

Nennen Sie die neue Methode aus den Methoden `Insert` und `Update`:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample9.cs)]

Fügen Sie in *ISchoolRepository.cs*die folgende Deklaration für die neue Datenzugriffs Methode hinzu:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample10.cs)]

Fügen Sie in *SchoolRepository.cs*die folgende `using`-Anweisung hinzu:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample11.cs)]

Fügen Sie in *SchoolRepository.cs*die folgende neue Datenzugriffs Methode hinzu:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample12.cs)]

Dieser Code ruft `Department` Entitäten ab, die über einen angegebenen Administrator verfügen. Es sollte nur eine Abteilung gefunden werden (sofern vorhanden). Da jedoch keine Einschränkung in die Datenbank integriert ist, ist der Rückgabetyp eine Auflistung für den Fall, dass mehrere Abteilungen gefunden werden.

Wenn der Objekt Kontext Entitäten aus der Datenbank abruft, werden diese standardmäßig im Objekt Zustands-Manager nachverfolgt. Der `MergeOption.NoTracking`-Parameter gibt an, dass diese Nachverfolgung für diese Abfrage nicht durchgeführt wird. Dies ist erforderlich, da die Abfrage möglicherweise die genaue Entität zurückgibt, die Sie aktualisieren möchten, und dann nicht mehr in der Lage sind, diese Entität anzufügen. Wenn Sie z. b. die Verlaufs Abteilung auf der Seite " *Departments. aspx* " Bearbeiten und den Administrator unverändert lassen, wird die Verlaufs Abteilung von dieser Abfrage zurückgegeben. Wenn `NoTracking` nicht festgelegt ist, verfügt der Objekt Kontext bereits über die Entität "Verlaufs Abteilung" in seinem Objekt Zustands-Manager. Wenn Sie dann die Entität der Verlaufs Abteilung, die neu erstellt wird, aus dem Ansichts Zustand anfügen, löst der Objekt Kontext eine Ausnahme aus, die `"An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key"`.

(Als Alternative zum Angeben von `MergeOption.NoTracking`können Sie nur für diese Abfrage einen neuen Objekt Kontext erstellen. Da der neue Objekt Kontext über einen eigenen Objekt Zustands-Manager verfügen würde, würde beim aufzurufen der `Attach`-Methode kein Konflikt auftreten. Der neue Objekt Kontext würde Metadaten und die Datenbankverbindung mit dem ursprünglichen Objekt Kontext gemeinsam verwenden, sodass die Leistungseinbußen dieses alternativen Ansatzes minimal wäre. Der hier gezeigte Ansatz führt jedoch die `NoTracking`-Option ein, die Sie in anderen Kontexten nützlich finden. Die `NoTracking`-Option wird in einem späteren Tutorial dieser Reihe ausführlicher erläutert.)

Fügen Sie im Testprojekt die neue Datenzugriffs Methode zu *MockSchoolRepository.cs*hinzu:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample13.cs)]

In diesem Code wird LINQ verwendet, um die gleiche Datenauswahl auszuführen, die das `ContosoUniversity` projektrepository LINQ to Entities für verwendet.

Führen Sie das Testprojekt erneut aus. Dieses Mal werden die Tests bestanden.

[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image17.png)

## <a name="handling-objectdatasource-exceptions"></a>Behandeln von ObjectDataSource-Ausnahmen

Führen Sie im `ContosoUniversity` Projekt die Seite " *Departments. aspx* " aus, und versuchen Sie, den Administrator für eine Abteilung zu einer Person zu wechseln, die bereits für eine andere Abteilung Administrator ist. (Beachten Sie, dass Sie nur die Abteilungen bearbeiten können, die Sie in diesem Tutorial hinzugefügt haben, da die Datenbank mit ungültigen Daten vorab geladen wird.) Sie erhalten die folgende Server Fehlerseite:

[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image19.png)

Sie möchten nicht, dass Benutzer diese Art von Fehlerseite sehen, daher müssen Sie Fehler Behandlungs Code hinzufügen. Öffnen Sie " *Departments. aspx* ", und geben Sie einen Handler für das `OnUpdated`-Ereignis des `DepartmentsObjectDataSource`an. Das `ObjectDataSource` öffnende Tag ähnelt nun dem folgenden Beispiel.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample14.aspx)]

Fügen Sie in *Departments.aspx.cs*die folgende `using`-Anweisung hinzu:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample15.cs)]

Fügen Sie den folgenden Handler für das `Updated`-Ereignis hinzu:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample16.cs)]

Wenn das `ObjectDataSource` Steuerelement eine Ausnahme abfängt, wenn es versucht, das Update auszuführen, übergibt es die Ausnahme im Ereignis Argument (`e`) an diesen Handler. Der Code im Handler prüft, ob es sich bei der Ausnahme um die doppelte Administrator Ausnahme handelt. Wenn dies der Fall ist, erstellt der Code ein Validierungs Steuerelement, das eine Fehlermeldung enthält, die vom `ValidationSummary` Steuerelement angezeigt werden soll.

Führen Sie die Seite aus, und versuchen Sie, jemanden erneut als Administrator zweier Abteilungen zu erstellen. Dieses Mal zeigt das `ValidationSummary`-Steuerelement eine Fehlermeldung an.

[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image21.png)

Nehmen Sie ähnliche Änderungen an der Seite *departmentsadd. aspx* vor. Geben *Sie in departmentsadd. aspx*einen Handler für das `OnInserted`-Ereignis des `DepartmentsObjectDataSource`an. Das resultierende Markup ähnelt dem folgenden Beispiel.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample17.aspx)]

Fügen Sie in *DepartmentsAdd.aspx.cs*dieselbe `using`-Anweisung hinzu:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample18.cs)]

Fügen Sie den folgenden Ereignishandler hinzu:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample19.cs)]

Sie können nun die Seite " *DepartmentsAdd.aspx.cs* " testen, um sicherzustellen, dass die Versuche, eine Person als Administrator mehrerer Abteilungen einzurichten, auch ordnungsgemäß verarbeitet werden.

Dadurch wird die Einführung in die Implementierung des Repository-Musters für die Verwendung des `ObjectDataSource`-Steuer Elements mit dem Entity Framework vervollständigt. Weitere Informationen über das Repository-Muster und die Testability finden Sie im MSDN-Whitepaper [Testability und Entity Framework 4,0](https://msdn.microsoft.com/library/ff714955.aspx).

Im folgenden Tutorial erfahren Sie, wie Sie der Anwendung Sortier-und Filterfunktionen hinzufügen.

> [!div class="step-by-step"]
> [Zurück](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)
> [Weiter](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)
