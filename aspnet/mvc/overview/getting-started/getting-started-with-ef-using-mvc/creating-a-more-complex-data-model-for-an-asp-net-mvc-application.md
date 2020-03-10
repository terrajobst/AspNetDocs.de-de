---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
title: 'Tutorial: Erstellen eines komplexeren Datenmodells für eine ASP.NET MVC-App'
author: tdykstra
description: In diesem Tutorial fügen Sie weitere Entitäten und Beziehungen hinzu und passen das Datenmodell an, indem Sie Formatierungs-, Validierungs-und Daten Bank Zuordnungsregeln angeben.
ms.author: riande
ms.date: 01/22/2019
ms.topic: tutorial
ms.assetid: 46f7f3c9-274f-4649-811d-92222a9b27e2
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 933354b09eb4674e6f523f8a65816410521f026f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78471291"
---
# <a name="tutorial-create-a-more-complex-data-model-for-an-aspnet-mvc-app"></a>Tutorial: Erstellen eines komplexeren Datenmodells für eine ASP.NET MVC-App

In den vorherigen Tutorials haben Sie mit einem einfachen Datenmodell gearbeitet, das aus drei Entitäten besteht. In diesem Tutorial fügen Sie weitere Entitäten und Beziehungen hinzu und passen das Datenmodell an, indem Sie Formatierungs-, Validierungs-und Daten Bank Zuordnungsregeln angeben. Dieser Artikel zeigt zwei Möglichkeiten zum Anpassen des Datenmodells: durch Hinzufügen von Attributen zu Entitäts Klassen und durch Hinzufügen von Code zur Daten Bank Kontext Klasse.

Wenn Sie dies erledigt haben, bilden die Entitätsklassen ein vollständiges Datenmodell, das in der folgenden Abbildung dargestellt wird:

![School_class_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image1.png)

In diesem Tutorial führen Sie Folgendes durch:

> [!div class="checklist"]
> * Anpassen des Datenmodells
> * Student-Entität aktualisieren
> * Erstellen der Entität „Instructor“
> * Erstellen der Entität „OfficeAssignment“
> * Ändern der Course-Entität
> * Erstellen der Entität „Department“
> * Ändern der Entität „Enrollment“
> * Hinzufügen von Code zum Daten Bank Kontext
> * Füllen der Datenbank mit Testdaten
> * Hinzufügen einer Migration
> * Aktualisieren der Datenbank

## <a name="prerequisites"></a>Voraussetzungen

* [Code First Migrationen und Bereitstellung](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="customize-the-data-model"></a>Anpassen des Datenmodells

In diesem Abschnitt wird das Anpassen des Datenmodells mithilfe von Attributen erläutert, die Regeln zur Formatierung, Validierung und Datenbankzuordnung angeben. In einigen der folgenden Abschnitte erstellen Sie das gesamte `School` Datenmodell, indem Sie den bereits erstellten Klassenattribute hinzufügen und neue Klassen für die verbleibenden Entitäts Typen im Modell erstellen.

### <a name="the-datatype-attribute"></a>Das DataType-Attribut

Für die Anmeldedaten von Studenten zeigen alle Webseiten derzeit die Zeit und das Datum an, obwohl für dieses Feld nur das Datum relevant ist. Indem Sie Attribute für die Datenanmerkung verwenden, können Sie eine Codeänderungen vornehmen, durch die das Anzeigeformat in jeder Ansicht korrigiert wird, in der die Daten angezeigt werden. Sie können dies beispielhaft testen, indem Sie ein Attribut zur `EnrollmentDate`-Eigenschaft der `Student`-Klasse hinzufügen.

Fügen Sie in *models\student.cs*eine `using`-Anweisung für den `System.ComponentModel.DataAnnotations`-Namespace hinzu, und fügen Sie der `EnrollmentDate`-Eigenschaft `DataType` und `DisplayFormat` Attribute hinzu, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,12-13)]

Das [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) -Attribut wird verwendet, um einen Datentyp anzugeben, der spezifischer als der Daten Bank interne Typ ist. In diesem Fall soll nur das Datum verfolgt werden, nicht das Datum und die Zeit. Die [DataType-Enumeration](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) stellt viele Datentypen bereit, wie z. b. *Datum, Uhrzeit, PhoneNumber, Currency, EmailAddress* usw. Das `DataType`-Attribut kann der Anwendung auch ermöglichen, typspezifische Features bereitzustellen. Beispielsweise kann ein `mailto:` Link für [DataType. EmailAddress](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)erstellt werden, und für [DataType. Date](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) in Browsern, die [HTML5](http://html5.org/)unterstützen, kann eine Datumsauswahl angegeben werden. Die [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) -Attribute gibt HTML 5-Attribute aus, die von HTML 5 [-](http://ejohn.org/blog/html-5-data-attributes/) Browsern verstanden werden können. Die [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) -Attribute bieten keine Validierung.

`DataType.Date` gibt nicht das Format des Datums an, das angezeigt wird. Standardmäßig wird das Datenfeld gemäß den Standardformaten basierend auf der [CultureInfo](https://msdn.microsoft.com/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx)des Servers angezeigt.

Das `DisplayFormat`-Attribut dient zum expliziten Angeben des Datumsformats:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample2.cs)]

Die `ApplyFormatInEditMode` Einstellung gibt an, dass die angegebene Formatierung auch angewendet werden soll, wenn der Wert zur Bearbeitung in einem Textfeld angezeigt wird. (Möglicherweise möchten Sie das Währungssymbol für einige Felder nicht ändern – z. b. für Währungswerte ist es möglicherweise nicht erforderlich, dass das Währungssymbol im Textfeld bearbeitet wird.)

Sie können das [Display Format](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) -Attribut eigenständig verwenden, aber es ist in der Regel eine gute Idee, auch das [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) -Attribut zu verwenden. Das `DataType`-Attribut übermittelt die *Semantik* der Daten im Gegensatz zum Rendering auf einem Bildschirm und bietet die folgenden Vorteile, die Sie mit `DisplayFormat`nicht erhalten:

- Der Browser kann HTML5-Features aktivieren (z.B. zum Anzeigen eines Kalendersteuerelements, des dem Gebietsschema entsprechenden Währungssymbols, von E-Mail-Links, einigen clientseitigen Eingabevalidierungen usw.).
- Standardmäßig wird der Browserdaten basierend [auf Ihrem Gebiets](https://msdn.microsoft.com/library/vstudio/wyzd2bce.aspx)Schema im richtigen Format Rendering.
- Das [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) -Attribut kann MVC ermöglichen, die richtige Feld Vorlage zum renderingder Daten auszuwählen ( [Display Format](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) verwendet die Zeichen folgen Vorlage). Weitere Informationen finden Sie in den Vorlagen von Brad Wilson [ASP.NET MVC 2](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). (Obwohl für MVC 2 geschrieben, gilt dieser Artikel weiterhin für die aktuelle Version von ASP.NET MVC.)

Wenn Sie das `DataType`-Attribut mit einem Datumsfeld verwenden, müssen Sie auch das `DisplayFormat`-Attribut angeben, um sicherzustellen, dass das Feld ordnungsgemäß in Chrome-Browsern gerendert wird. Weitere Informationen finden Sie in [diesem StackOverflow-Thread](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie).

Weitere Informationen zum Umgang mit anderen Datumsformaten in MVC finden Sie unter [MVC 5 Introduction: Überprüfen der Bearbeitungsmethoden und Bearbeiten der Ansicht](../introduction/examining-the-edit-methods-and-edit-view.md) und suchen auf der Seite &quot;Internationalisierungs&quot;.

Führen Sie die Index Seite "Student" erneut aus, und beachten Sie, dass die Zeiten für die Registrierungsdaten nicht mehr angezeigt werden. Das gleiche gilt für jede Ansicht, die das `Student` Modell verwendet.

![Students_index_page_with_formatted_date](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image2.png)

### <a name="the-stringlengthattribute"></a>Das StringLengthAttribute

Sie können ebenfalls Regeln für die Datenvalidierung und Meldungen für Validierungsfehler mithilfe von Attributen angeben. Das [StringLength-Attribut](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) legt die maximale Länge in der Datenbank fest und bietet Client seitige und serverseitige Validierung für ASP.NET MVC. Sie können ebenfalls die mindestens erforderliche Zeichenfolgenlänge in diesem Attribut angeben, dieser Wert hat jedoch keine Auswirkung auf das Datenbankschema.

Angenommen, Sie möchten sicherstellen, dass Benutzer nicht mehr als 50 Zeichen für einen Namen eingeben. Fügen Sie zum Hinzufügen dieser Einschränkung den Eigenschaften `LastName` und `FirstMidName` [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) -Attribute hinzu, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=10,12)]

Das [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) -Attribut verhindert nicht, dass ein Benutzer Leerraum für einen Namen eingibt. Mit dem [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) -Attribut können Sie Einschränkungen auf die Eingabe anwenden. Der folgende Code erfordert beispielsweise, dass das erste Zeichen in Großbuchstaben und die restlichen Zeichen alphabetisch sind:

`[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]`

Das Attribut " [MaxLength](https://msdn.microsoft.com/library/System.ComponentModel.DataAnnotations.MaxLengthAttribute.aspx) " stellt eine ähnliche Funktionalität wie das [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) -Attribut bereit, stellt jedoch keine Client seitige Validierung bereit.

Führen Sie die Anwendung aus, und klicken Sie auf die Registerkarte **Studenten** Sie erhalten die folgende Fehlermeldung:

*Das Modell, das den "schoolContext"-Kontext unterstützt, wurde seit der Erstellung der Datenbank geändert. Verwenden Sie Code First-Migrationen, um die Datenbank zu aktualisieren ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269)).*

Das Datenbankmodell hat sich auf eine Weise geändert, die eine Änderung des Datenbankschemas erfordert, und Entity Framework erkannte. Sie verwenden Migrationen, um das Schema zu aktualisieren, ohne Daten zu verlieren, die Sie der Datenbank mithilfe der Benutzeroberfläche hinzugefügt haben. Wenn Sie Daten geändert haben, die von der `Seed`-Methode erstellt wurden, werden diese aufgrund der [addorupdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx) -Methode, die Sie in der `Seed`-Methode verwenden, wieder in ihren ursprünglichen Zustand geändert. ([Addorupdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx) entspricht einem Upsert-Vorgang aus der Daten Bank Terminologie.)

Geben Sie die folgenden Befehle in die Paket-Manager-Konsole ein:

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample4.cmd)]

Mit dem `add-migration`-Befehl wird eine Datei mit dem Namen *&lt;Timestamp&gt;\_MaxLengthOnNames.cs*erstellt. Diese Datei enthält Code in der `Up`-Methode, durch den die Datenbank dem aktuellen Datenmodell entsprechend aktualisiert wird. Der Code wurde durch den `update-database`-Befehl ausgeführt.

Der Zeitstempel, der dem Migrations Dateinamen vorangestellt wird, wird von Entity Framework verwendet, um die Migrationen zu sortieren. Sie können mehrere Migrationen erstellen, bevor Sie den `update-database` Befehl ausführen. Anschließend werden alle Migrationen in der Reihenfolge angewendet, in der Sie erstellt wurden.

Führen Sie die Seite **Erstellen** aus, und geben Sie einen Namen mit mehr als 50 Zeichen ein. Wenn Sie auf **Erstellen**klicken, zeigt die Client seitige Validierung eine Fehlermeldung an: *das Feld "LastName" muss eine Zeichenfolge mit einer maximalen Länge von 50 sein.*

### <a name="the-column-attribute"></a>Das Column-Attribut

Sie können ebenfalls Attribute verwenden, um zu steuern, wie Ihre Klassen und Eigenschaften der Datenbank zugeordnet werden. Angenommen, Sie haben den Namen `FirstMidName` für das Feld „first-name“ verwendet, da das Feld ebenfalls einen Zweitnamen enthalten kann. Sie möchten jedoch, dass die Datenbankspalte in `FirstName` umbenannt wird, damit Benutzer, die Ad-hob-Abfragen für die Datenbank schreiben, an diesen Namen gewöhnt sind. Sie können das `Column`-Attribut verwenden, um diese Zuordnung durchzuführen.

Das `Column`-Attribut gibt an, dass bei der Erstellung der Datenbank die Spalte des `Student`-Elements, die der `FirstMidName`-Eigenschaft zugeordnet ist, den Namen `FirstName` erhält. Wenn Ihr Code also auf `Student.FirstMidName` verweist, stammen die Daten aus der `FirstName`-Spalte der `Student`-Tabelle oder werden in dieser aktualisiert. Wenn Sie keine Spaltennamen angeben, erhalten Sie denselben Namen wie der Eigenschaftsname.

Fügen Sie in der Datei *Student.cs* eine `using`-Anweisung für [System. ComponentModel. DataAnnotations. Schema](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.aspx) hinzu, und fügen Sie das Column Name-Attribut der `FirstMidName`-Eigenschaft hinzu, wie im folgenden hervorgehobenen Code gezeigt:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample5.cs?highlight=4,14)]

Durch das Hinzufügen des [Column-Attributs](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx) wird das Modell geändert, das den schoolContext sichert, sodass es nicht mit der Datenbank identisch ist. Geben Sie die folgenden Befehle in der PMC ein, um eine weitere Migration zu erstellen:

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample6.cmd)]

Öffnen Sie in **Server-Explorer**den Tabellen-Designer für *Schüler* und Studenten, indem Sie auf die Tabelle *Student* doppelklicken.

Die folgende Abbildung zeigt den ursprünglichen Spaltennamen wie vor dem Anwenden der ersten beiden Migrationen. Neben dem Spaltennamen, der von `FirstMidName` in `FirstName`geändert wird, haben sich die beiden namens Spalten von `MAX` Länge in 50 Zeichen geändert.

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image5.png)

Sie können auch Änderungen an der Daten Bank Zuordnung mithilfe der [flüssigen API](https://msdn.microsoft.com/data/jj591617)vornehmen, wie Sie später in diesem Tutorial sehen werden.

> [!NOTE]
> Wenn Sie vor dem Erstellen aller Entitätsklassen in den folgenden Abschnitten versuchen, zu kompilieren, werden möglicherweise Compilerfehler angezeigt.

## <a name="update-student-entity"></a>Student-Entität aktualisieren

Ersetzen Sie in *models\student.cs*den Code, den Sie zuvor hinzugefügt haben, durch den folgenden Code. Die Änderungen werden hervorgehoben.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample7.cs?highlight=11,13,15,18,22,25-32)]

### <a name="the-required-attribute"></a>Das erforderliche Attribut.

Durch das [erforderliche Attribut](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) werden die namens Eigenschaften als Pflichtfelder angegeben. Der `Required attribute` ist für Werttypen wie DateTime, int, Double und float nicht erforderlich. Werttypen kann kein NULL-Wert zugewiesen werden, sodass Sie grundsätzlich als Pflichtfelder behandelt werden. 

Das `Required`-Attribut muss mit `MinimumLength` verwendet werden, damit die `MinimumLength` erzwungen wird.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample8.cs?highlight=2)]

`MinimumLength` und `Required` lassen Leerzeichen zu, um die Überprüfung zu bestehen. Verwenden Sie das `RegularExpression`-Attribut für die vollständige Kontrolle über die Zeichenfolge.

### <a name="the-display-attribute"></a>Das Anzeige Attribut

Das `Display`-Attribut gibt an, dass die Beschriftung der Textfelder in jeder Instanz „First Name“, „Last Name“, „Full Name“ und „Enrollment Date“ lauten soll, statt dem Eigenschaftsnamen zu entsprechen (bei dem die Wörter nicht durch Leerräume getrennt werden).

### <a name="the-fullname-calculated-property"></a>Die berechnete FullName-Eigenschaft.

Bei `FullName` handelt es sich um eine berechnete Eigenschaft, die einen Wert zurückgibt, der durch das Verketten von zwei weiteren Eigenschaften erstellt wird. Daher verfügt sie nur über einen `get` Accessor, und in der Datenbank wird keine `FullName` Spalte generiert.

## <a name="create-instructor-entity"></a>Erstellen der Entität „Instructor“

Erstellen Sie *models\instructor .cs*, und ersetzen Sie den Vorlagen Code durch den folgenden Code:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample9.cs)]

Beachten Sie, dass einige Eigenschaften in den Entitäten `Student` und `Instructor` identisch sind. Im folgenden Tutorial [Implementing Inheritance (Implementierung von Vererbung)](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md) gestalten Sie diesen Code um, um die Redundanzen zu entfernen.

Sie können mehrere Attribute in einer Zeile platzieren, sodass Sie auch die Klasse Instructor wie folgt schreiben können:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

### <a name="the-courses-and-officeassignment-navigation-properties"></a>Die Navigations Eigenschaften "Kurse" und "officezuweisung"

Bei den Eigenschaften `Courses` und `OfficeAssignment` handelt es sich um Navigationseigenschaften. Wie bereits erwähnt, sind Sie in der Regel als [virtuell](https://msdn.microsoft.com/library/9fkccyh4(v=vs.110).aspx) definiert, sodass Sie eine Entity Framework Funktion mit dem Namen [Lazy Loading](https://msdn.microsoft.com/magazine/hh205756.aspx)nutzen können. Wenn eine Navigations Eigenschaft mehrere Entitäten enthalten kann, muss der Typ außerdem die [ICollection&lt;t&gt;](https://msdn.microsoft.com/library/92t2ye13.aspx) -Schnittstelle implementieren. Beispielsweise ist [IList&lt;t&gt;](https://msdn.microsoft.com/library/5y536ey6.aspx) qualifiziert, aber nicht [IEnumerable&lt;t&gt;](https://msdn.microsoft.com/library/9eekhta0.aspx) , weil `IEnumerable<T>` [Add](https://msdn.microsoft.com/library/63ywd54z.aspx)nicht implementiert.

Ein Dozent kann eine beliebige Anzahl von Kursen unterrichten, sodass `Courses` als eine Auflistung von `Course` Entitäten definiert ist.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

Unser Geschäftsregel Status ein Dozent kann nur über höchstens ein Büro verfügen, sodass `OfficeAssignment` als einzelne `OfficeAssignment` Entität definiert ist (was `null` werden kann, wenn kein Büro zugewiesen ist).

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

## <a name="create-officeassignment-entity"></a>Erstellen der Entität „OfficeAssignment“

Erstellen Sie *models\officezuordment.cs* mit dem folgenden Code:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

Erstellen Sie das Projekt, das Ihre Änderungen speichert und überprüft, ob Sie keine Kopier-und Einfüge Fehler vorgenommen haben, die der Compiler erfassen kann.

### <a name="the-key-attribute"></a>Das Schlüssel Attribut

Zwischen den `Instructor` und den `OfficeAssignment` Entitäten besteht eine 1:0-oder 1-Beziehung. Eine Büro Zuweisung ist nur in Bezug auf den Dozenten vorhanden, dem Sie zugewiesen ist. Deshalb ist der Primärschlüssel auch der Fremdschlüssel für die `Instructor` Entität. Allerdings kann der Entity Framework `InstructorID` nicht automatisch als Primärschlüssel dieser Entität erkennen, weil der Name nicht der `ID`-oder *className* -`ID` Benennungs Konvention folgt. Deshalb wird das `Key`-Attribut verwendet, um „InstructorID“ als Schlüssel zu erkennen:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample14.cs)]

Sie können auch das `Key`-Attribut verwenden, wenn die Entität über einen eigenen Primärschlüssel verfügt, Sie aber eine andere Eigenschaft als `classnameID` oder `ID`benennen möchten. Standardmäßig behandelt EF den Schlüssel als nicht-Daten Bank generiert, da die Spalte für eine identifizierende Beziehung vorgesehen ist.

### <a name="the-foreignkey-attribute"></a>Das Fremdschlüssel Attribut.

Wenn eine 1: NULL-oder eine 1-zu-eins-Beziehung zwischen zwei Entitäten vorliegt (z. b. zwischen `OfficeAssignment` und `Instructor`), kann EF nicht herausfinden, welches Ende der Beziehung der Prinzipal ist und welches Ende abhängig ist. Eine-zu-eins-Beziehung hat in jeder Klasse eine Verweis Navigations Eigenschaft für die andere Klasse. Das fremd [Schlüssel Attribut](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx) kann auf die abhängige Klasse angewendet werden, um die Beziehung herzustellen. Wenn Sie das fremd [Schlüssel Attribut](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx)weglassen, erhalten Sie die folgende Fehlermeldung, wenn Sie versuchen, die Migration zu erstellen:

*Das Prinzipal Ende einer Zuordnung zwischen den Typen "contosouniversity. Models. officezuweisung" und "contosouniversity. Models. Instructor" kann nicht bestimmt werden. Das Prinzipal Ende dieser Zuordnung muss entweder mithilfe der vertrauenden API der Beziehung oder mit Daten Anmerkungen explizit konfiguriert werden.*

Später in diesem Tutorial erfahren Sie, wie Sie diese Beziehung mit der flüssigen API konfigurieren.

### <a name="the-instructor-navigation-property"></a>Die Instructor-Navigations Eigenschaft

Die `Instructor`-Entität verfügt über eine `OfficeAssignment` Navigations Eigenschaft, die NULL-Werte zulässt (weil ein Dozenten möglicherweise keine Office `InstructorID`-Zuweisung hat), und die `OfficeAssignment`-Entität verfügt über eine `Instructor` Navigations Eigenschaft, die nicht auf NULL festgelegt werden kann (da eine Office-Zuweisung ohne einen Dozenten nicht vorhanden sein kann). Wenn eine `Instructor` Entität über eine verknüpfte `OfficeAssignment` Entität verfügt, verfügt jede Entität in der zugehörigen Navigations Eigenschaft über einen Verweis auf die andere Entität.

Sie können ein `[Required]`-Attribut in der Instructor-Navigations Eigenschaft ablegen, um anzugeben, dass es sich um einen zugehörigen Dozenten handeln muss, aber Sie müssen dies nicht tun, da der InstructorID-Fremdschlüssel (der auch der Schlüssel für diese Tabelle ist) keine NULL-Werte zulässt.

## <a name="modify-the-course-entity"></a>Ändern der Course-Entität

Ersetzen Sie in *models\course.cs*den Code, den Sie zuvor hinzugefügt haben, durch den folgenden Code:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

Die Course-Entität verfügt über eine Fremdschlüssel Eigenschaft `DepartmentID` die auf die verknüpfte `Department` Entität verweist und über eine `Department` Navigations Eigenschaft verfügt. Entity Framework erfordert nicht, dass Sie eine Fremdschlüsseleigenschaft zu Ihrem Datenmodell hinzufügen, wenn eine Navigationseigenschaft für eine verknüpfte Entität vorhanden ist. EF erstellt automatisch Fremdschlüssel in der Datenbank, wo Sie benötigt werden. Durch Fremdschlüssel im Datenmodell können Updates einfacher und effizienter durchgeführt werden. Wenn Sie z. b. eine zu bearbeitende Kurs Entität abrufen, ist die `Department` Entität NULL, wenn Sie Sie nicht laden. Wenn Sie also die Course-Entität aktualisieren, müssen Sie zuerst die `Department` Entität abrufen. Wenn die Fremdschlüssel Eigenschaft `DepartmentID` im Datenmodell enthalten ist, müssen Sie die `Department` Entität vor dem Aktualisieren nicht abrufen.

### <a name="the-databasegenerated-attribute"></a>Das databasegenerated-Attribut

Das [databasegenerated-Attribut](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute.aspx) mit dem [None](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.110).aspx) -Parameter für die `CourseID`-Eigenschaft gibt an, dass Primärschlüssel Werte vom Benutzer bereitgestellt werden, anstatt von der Datenbank generiert zu werden.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

Standardmäßig geht der Entity Framework davon aus, dass Primärschlüssel Werte von der Datenbank generiert werden. Das ist in den meisten Fällen erwünscht. Für `Course` Entitäten verwenden Sie jedoch eine benutzerdefinierte Kursnummer, z. b. eine 1000-Serie für eine Abteilung, eine 2000-Serie für eine andere Abteilung usw.

### <a name="foreign-key-and-navigation-properties"></a>Fremdschlüssel-und Navigations Eigenschaften

Die Fremdschlüssel Eigenschaften und Navigations Eigenschaften in der `Course` Entität reflektieren die folgenden Beziehungen:

- Ein Kurs ist einer Abteilung zugeordnet, sodass es aus den oben genannten Gründen eine `DepartmentID`-Fremdschlüsseleigenschaft und eine `Department`-Navigationseigenschaft gibt.

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]
- In einem Kurs können beliebig viele Studenten angemeldet sein, darum stellt die `Enrollments`-Navigationseigenschaft eine Auflistung dar:

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample18.cs)]
- Ein Kurs kann von mehreren Dozenten unterrichtet werden, darum stellt die `Instructors`-Navigationseigenschaft eine Auflistung dar:

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample19.cs)]

## <a name="create-the-department-entity"></a>Erstellen der Entität „Department“

Erstellen Sie *models\department.cs* mit dem folgenden Code:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample20.cs)]

### <a name="the-column-attribute"></a>Das Column-Attribut

Zuvor haben Sie das [Column-Attribut](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx) verwendet, um die Zuordnung von Spaltennamen zu ändern. Im Code für die `Department`-Entität wird das `Column`-Attribut verwendet, um die SQL-Datentyp Zuordnung zu ändern, sodass die Spalte mit dem SQL Server [Money](https://msdn.microsoft.com/library/ms179882.aspx) -Typ in der Datenbank definiert wird:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample21.cs)]

Die Spalten Zuordnung ist im Allgemeinen nicht erforderlich, da die Entity Framework in der Regel den entsprechenden SQL Server Datentyp basierend auf dem CLR-Typ auswählt, den Sie für die Eigenschaft definieren. Der CLR-Typ `decimal` wird dem SQL Server-Typ `decimal` zugeordnet. In diesem Fall wissen Sie jedoch, dass die Spalte Währungs Beträge enthält, und der [Money](https://msdn.microsoft.com/library/ms179882.aspx) -Datentyp ist besser geeignet. Weitere Informationen zu CLR-Datentypen und deren Abgleich mit SQL Server-Datentypen finden Sie unter [SqlClient für Entity frameworktypes](https://msdn.microsoft.com/library/bb896344.aspx).

### <a name="foreign-key-and-navigation-properties"></a>Fremdschlüssel-und Navigations Eigenschaften

Die Fremdschlüssel- und Navigationseigenschaften stellen folgende Beziehungen dar:

- Eine Abteilung kann einen Administrator besitzen oder nicht, und bei einem Administrator handelt es sich immer um einen Dozenten. Daher ist die `InstructorID`-Eigenschaft als Fremdschlüssel für die `Instructor` Entität enthalten, und ein Fragezeichen wird nach der `int` Type-Bezeichnung hinzugefügt, um die Eigenschaft als Nullable zu markieren. Die Navigations Eigenschaft hat den Namen `Administrator`, enthält jedoch eine `Instructor` Entität:

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample22.cs)]
- Eine Abteilung kann viele Kurse haben, sodass es eine `Courses` Navigations Eigenschaft gibt:

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample23.cs)]

  > [!NOTE]
  > Gemäß der Konvention aktiviert Entity Framework das kaskadierende Delete für nicht auf NULL festlegbare Fremdschlüssel und für m:n-Beziehungen. Dies kann zu zirkulären Regeln für kaskadierende Deletes führen, wodurch eine Ausnahme ausgelöst wird, wenn Sie versuchen, eine Migration hinzuzufügen. Wenn Sie die `Department.InstructorID`-Eigenschaft beispielsweise nicht als Nullable definiert haben, erhalten Sie die folgende Ausnahme Meldung: "die referenzielle Beziehung führt zu einem zyklischen Verweis, der nicht zulässig ist." Wenn Ihre Geschäftsregeln `InstructorID` Eigenschaft erfordern, dass Sie keine NULL-Werte zulässt, müssten Sie die folgende fließende API-Anweisung verwenden, um die Lösch Weitergabe für die Beziehung zu deaktivieren:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample24.cs)]

## <a name="modify-the-enrollment-entity"></a>Ändern der Entität „Enrollment“

 Ersetzen Sie in *models\registriment.cs*den Code, den Sie zuvor hinzugefügt haben, durch den folgenden Code:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample25.cs?highlight=1,15)]

### <a name="foreign-key-and-navigation-properties"></a>Fremdschlüssel-und Navigations Eigenschaften

Die Fremdschlüssel- und Navigationseigenschaften stellen folgende Beziehungen dar:

- Ein Anmeldungsdatensatz gilt für einen einzelnen Kurs, sodass es eine `CourseID`-Fremdschlüsseleigenschaft und eine `Course`-Navigationseigenschaft gibt:

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample26.cs)]
- Ein Anmeldungsdatensatz gilt für einen einzelnen Studenten, sodass es eine `StudentID`-Fremdschlüsseleigenschaft und eine `Student`-Navigationseigenschaft gibt:

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample27.cs)]

### <a name="many-to-many-relationships"></a>m:n-Beziehungen

Es gibt eine m:n-Beziehung zwischen den Entitäten `Student` und `Course`, und die `Enrollment` Entität fungiert als m:n-jointabelle *mit Nutzlast* in der Datenbank. Dies bedeutet, dass die `Enrollment` Tabelle zusätzliche Daten neben Fremdschlüsseln für die verbundenen Tabellen enthält (in diesem Fall ein Primärschlüssel und eine `Grade`-Eigenschaft).

Die folgende Abbildung stellt dar, wie diese Beziehungen in einem Entitätsdiagramm aussehen. (Dieses Diagramm wurde mithilfe der [Entity Framework Power Tools](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d)generiert; das Erstellen des Diagramms ist nicht Teil des Tutorials, sondern wird hier nur als Abbildung verwendet.)

![Student-Course_many-many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image12.png)

Jede Beziehungs Linie hat ein 1 an einem Ende und ein Sternchen (\*) am anderen, das eine 1: n-Beziehung angibt.

Wenn die `Enrollment` Tabelle keine Informationen zur Qualität enthielt, muss Sie nur die beiden Fremdschlüssel `CourseID` und `StudentID`enthalten. In diesem Fall würde es sich um eine m:n-jointabelle *ohne Nutzlast* (oder eine *reine jointabelle*) in der Datenbank beziehen, und Sie müssten überhaupt keine Modell Klasse erstellen. Die Entitäten `Instructor` und `Course` haben diese Art von m:n-Beziehung, und wie Sie sehen können, gibt es keine Entitäts Klasse zwischen Ihnen:

![Instructor-Course_many-many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image13.png)

In der Datenbank ist jedoch eine jointabelle erforderlich, wie im folgenden Daten Bank Diagramm gezeigt:

![Instructor-Course_many-many_relationship_tables](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image14.png)

Der Entity Framework erstellt automatisch die `CourseInstructor` Tabelle, und Sie lesen und aktualisieren diese indirekt, indem Sie die `Instructor.Courses` und `Course.Instructors` Navigations Eigenschaften lesen und aktualisieren.

## <a name="entity-relationship-diagram"></a>Entitäts Beziehungs Diagramm

Die folgende Abbildung stellt das Diagramm dar, das von Entity Framework Power Tools für das vollständige Modell „School“ erstellt wird.

![School_data_model_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image1.png)

Neben den m:n-Beziehungslinien (\* \*) und den 1: n-Beziehungs Linien (1 bis \*) Sie sehen hier die 1: NULL-oder eine Beziehungs Linie (1 bis 0.. 1) zwischen den Entitäten "`Instructor`" und "`OfficeAssignment`" und der NULL-oder 1: n-Beziehungs Linie (0.. 1 bis \*) zwischen den Entitäten "Instructor" und "Department".

## <a name="add-code-to-database-context"></a>Hinzufügen von Code zum Daten Bank Kontext

Als Nächstes fügen Sie der `SchoolContext`-Klasse die neuen Entitäten hinzu und passen einige der zuder Zuordnung mithilfe von [fließenden API](https://msdn.microsoft.com/data/jj591617) -Aufrufen an. Die API ist "fließend", da Sie häufig verwendet wird, indem Sie eine Reihe von Methoden aufrufen zu einer einzigen Anweisung in einer einzigen Anweisung überspannen, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample28.cs)]

In diesem Tutorial verwenden Sie die fließende API nur für die Daten Bank Zuordnung, die Sie nicht mit Attributen ausführen können. Sie können die Fluent-API jedoch ebenfalls verwenden, um den Großteil der Regeln für die Formatierung, Validierung und Zuordnung anzugeben, die Sie mithilfe von Attributen festlegen können. Manche Attribute, z.B. `MinimumLength`, können nicht mit der Fluent-API angewendet werden. Wie bereits erwähnt, wird das Schema `MinimumLength` nicht geändert, sondern nur eine Client-und serverseitige Validierungs Regel angewendet.

Einige Entwickler bevorzugen die exklusive Verwendung der Fluent-API, um ihre Entitätsklassen „rein“ zu halten. Sie können Attribute und die Fluent-API verwenden, wenn Sie möchten. Einige Anpassungen können nur mithilfe der Fluent-API vorgenommen werden. Im Allgemeinen wird jedoch empfohlen, einen der beiden Ansätze auszuwählen und diesen so konsistent wie möglich zu verwenden.

Um dem Datenmodell die neuen Entitäten hinzuzufügen und die Daten Bank Zuordnung durchzuführen, die Sie nicht mithilfe von Attributen durchgeführt haben, ersetzen Sie den Code in " *dal\schoolcontext.cs* " durch den folgenden Code:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample29.cs)]

Die New-Anweisung in der [onmodelcreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) -Methode konfiguriert die m:n-jointabelle:

- Für die m:n-Beziehung zwischen den Entitäten `Instructor` und `Course` gibt der Code die Tabellen-und Spaltennamen für die jointabelle an. Code First können die m:n-Beziehung für Sie ohne diesen Code konfigurieren, aber wenn Sie Sie nicht nennen, erhalten Sie Standardnamen wie `InstructorInstructorID` für die `InstructorID` Spalte.

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample30.cs)]

Der folgende Code enthält ein Beispiel dafür, wie Sie eine fließende API anstelle von Attributen verwenden können, um die Beziehung zwischen den Entitäten `Instructor` und `OfficeAssignment` anzugeben:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample31.cs)]

Informationen dazu, welche "fließend API"-Anweisungen im Hintergrund ausgeführt werden, finden Sie im Blogbeitrag der [fließenden API](https://blogs.msdn.com/b/aspnetue/archive/2011/05/04/entity-framework-code-first-tutorial-supplement-what-is-going-on-in-a-fluent-api-call.aspx) .

## <a name="seed-database-with-test-data"></a>Füllen der Datenbank mit Testdaten

Ersetzen Sie den Code in der Datei *migrations\configuration.cs* durch den folgenden Code, um Seed-Daten für die neuen Entitäten bereitzustellen, die Sie erstellt haben.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample32.cs)]

Wie Sie im ersten Tutorial gesehen haben, aktualisiert der größte Teil dieses Codes einfach neue Entitäts Objekte und lädt Beispiel Daten in Eigenschaften, die für den Test erforderlich sind. Beachten Sie jedoch, dass die `Course`-Entität, die über eine m:n-Beziehung mit der `Instructor`-Entität verfügt, behandelt wird:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample33.cs)]

Wenn Sie ein `Course` Objekt erstellen, initialisieren Sie die `Instructors` Navigations Eigenschaft als leere Auflistung, indem Sie den Code-`Instructors = new List<Instructor>()`verwenden. Dies ermöglicht es, mithilfe der `Instructors.Add`-Methode `Instructor` Entitäten hinzuzufügen, die mit diesem `Course` verknüpft sind. Wenn Sie keine leere Liste erstellt haben, können Sie diese Beziehungen nicht hinzufügen, da die `Instructors`-Eigenschaft NULL wäre und keine `Add`-Methode haben würde. Sie können auch die Listen Initialisierung dem Konstruktor hinzufügen.

## <a name="add-a-migration"></a>Hinzufügen einer Migration

Geben Sie in der PMC den `add-migration` Befehl ein (führen Sie den `update-database`-Befehl noch nicht aus):

`add-Migration ComplexDataModel`

Wenn Sie versucht haben, den Befehl `update-database` zu diesem Zeitpunkt auszuführen (führen Sie diesen noch nicht aus), erhalten Sie folgende Fehlermeldung:

*Die ALTER TABLE-Anweisung steht in Konflikt mit der FOREIGN KEY-Einschränkung "FK\_dbo. Kurs\_dbo. Abteilung\_DepartmentID ". Der Konflikt trat in der Datenbank "Conto souniversity", Tabelle "dbo" auf. Department ", Spalte" DepartmentID ".*

Manchmal müssen Sie beim Ausführen von Migrationen mit vorhandenen Daten Stub-Daten in die Datenbank einfügen, um Foreign Key-Einschränkungen zu erfüllen, und das ist das, was Sie jetzt tun müssen. Der generierte Code in der complexdatamodel-`Up`-Methode fügt der `Course` Tabelle einen `DepartmentID` Fremdschlüssel hinzu, der keine NULL-Werte zulässt. Da in der `Course` Tabelle bereits Zeilen vorhanden sind, wenn der Code ausgeführt wird, schlägt der `AddColumn` Vorgang fehl, da SQL Server nicht weiß, welcher Wert in die Spalte eingefügt werden darf, der nicht NULL sein darf. Daher müssen Sie den Code ändern, um der neuen Spalte einen Standardwert zuzuweisen, und eine Stub-Abteilung mit dem Namen "Temp" erstellen, die als Standard Abteilung fungiert. Folglich werden vorhandene `Course` Zeilen nach dem Ausführen der `Up`-Methode mit der "Temp"-Abteilung verknüpft. Sie können Sie mit den richtigen Abteilungen in der `Seed`-Methode verknüpfen.

Bearbeiten Sie den &lt;*Zeitstempel-&gt;\_ComplexDataModel.cs* -Datei, kommentieren Sie die Codezeile aus, die die Spalte "DepartmentID" der Tabelle "Course" hinzufügt, und fügen Sie den folgenden hervorgehobenen Code hinzu (die kommentierte Zeile wird ebenfalls hervorgehoben):

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample34.cs?highlight=14-18)]

Wenn die `Seed`-Methode ausgeführt wird, werden Zeilen in die `Department` Tabelle eingefügt, und vorhandene `Course` Zeilen werden mit den neuen `Department` Zeilen verknüpft. Wenn Sie keine Kurse in der Benutzeroberfläche hinzugefügt haben, benötigen Sie die "Temp"-Abteilung oder den Standardwert für die `Course.DepartmentID` Spalte nicht mehr. Um die Möglichkeit zuzulassen, dass ein Benutzer möglicherweise Kurse mithilfe der Anwendung hinzugefügt hat, möchten Sie auch den Code der `Seed`-Methode aktualisieren, um sicherzustellen, dass alle `Course` Zeilen (nicht nur die, die von früheren Ausführungen der `Seed`-Methode eingefügt wurden) gültige `DepartmentID` Werte aufweisen, bevor Sie den Standardwert aus der Spalte entfernen und die Abteilung "Temp" löschen.

## <a name="update-the-database"></a>Aktualisieren der Datenbank

Nachdem Sie die &lt;*Zeitstempel-&gt;\_ComplexDataModel.cs* -Datei bearbeitet haben, geben Sie den `update-database` Befehl in der PMC ein, um die Migration auszuführen.

[!code-powershell[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample35.ps1)]

> [!NOTE]
> Beim Migrieren von Daten und vornehmen von Schema Änderungen können andere Fehler auftreten. Wenn Sie Migrationsfehler erhalten, die Sie nicht beheben können, können Sie den Datenbanknamen in der Verbindungszeichenfolge ändern oder die Datenbank löschen. Der einfachste Ansatz besteht darin, die Datenbank in der Datei " *Web. config* " umzubenennen. Das folgende Beispiel zeigt, dass der Name in Cu\_Test geändert wurde:
>
> [!code-xml[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample36.xml?highlight=1)]
>
> Bei einer neuen Datenbank gibt es keine zu migrierenden Daten, und der `update-database`-Befehl wird wahrscheinlich ohne Fehler ausgeführt. Anweisungen zum Löschen der Datenbank finden Sie unter Vorgehensweise beim Löschen [einer Datenbank aus Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).
>
> Wenn dies nicht möglich ist, können Sie versuchen, die Datenbank erneut zu initialisieren, indem Sie den folgenden Befehl in der PMC eingeben:
>
> `update-database -TargetMigration:0`

Öffnen Sie die Datenbank in **Server-Explorer** wie zuvor, und erweitern Sie den Knoten **Tabellen** , um zu sehen, dass alle Tabellen erstellt wurden. (Klicken Sie auf die Schaltfläche **Aktualisieren** , wenn Sie noch **Server-Explorer** bereits geöffnet haben.)

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image16.png)

Sie haben keine Modell Klasse für die `CourseInstructor` Tabelle erstellt. Wie bereits erwähnt, handelt es sich hierbei um eine jointabelle für die m:n-Beziehung zwischen den Entitäten `Instructor` und `Course`.

Klicken Sie mit der rechten Maustaste auf die Tabelle `CourseInstructor`, und wählen Sie **Tabellendaten anzeigen** aus, um zu überprüfen, ob diese Daten als Ergebnis der `Instructor` Entitäten enthalten, die Sie der `Course.Instructors` Navigations Eigenschaft hinzugefügt haben.

![Table_data_in_CourseInstructor_table](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image17.png)

## <a name="get-the-code"></a>Abrufen des Codes

[Herunterladen des abgeschlossenen Projekts](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Links zu anderen Entity Framework Ressourcen finden Sie unter [ASP.NET Data Access-Empfohlene Ressourcen](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial führen Sie Folgendes durch:

> [!div class="checklist"]
> * Anpassen des Datenmodells
> * Aktualisierte Student-Entität
> * Entität „Instructor“ wurde erstellt
> * Entität „OfficeAssignment“ wurde erstellt
> * Ändern der Course-Entität
> * Die Abteilungs Entität wurde erstellt.
> * Die Registrierungs Entität wurde geändert.
> * Hinzugefügter Code zum Daten Bank Kontext
> * Datenbank wurde mit Testdaten gefüllt
> * Migration wurde hinzugefügt
> * Datenbank wurde aktualisiert

Fahren Sie mit dem nächsten Artikel fort, um zu erfahren, wie Sie verwandte Daten lesen und anzeigen, die das Entity Framework in Navigations Eigenschaften lädt.

> [!div class="nextstepaction"]
> [Lesen relevanter Daten](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
