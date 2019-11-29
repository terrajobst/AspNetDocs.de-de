---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
title: Erstellen eines komplexeren Datenmodells für eine ASP.NET MVC-Anwendung (4 von 10) | Microsoft-Dokumentation
author: tdykstra
description: Die Beispiel-Webanwendung der Beispiel-Web-App veranschaulicht, wie ASP.NET MVC 4-Anwendungen mithilfe der Entity Framework 5 Code First und Visual Studio erstellt werden...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: f81f3d80-3674-4d8e-a9b1-87feed1a93c9
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 9c19ec47ad98a319ddee17db014c4b15e3734778
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74595362"
---
# <a name="creating-a-more-complex-data-model-for-an-aspnet-mvc-application-4-of-10"></a>Erstellen eines komplexeren Datenmodells für eine ASP.NET MVC-Anwendung (4 von 10)

von [Tom Dykstra](https://github.com/tdykstra)

[Herunterladen des abgeschlossenen Projekts](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Die Beispiel-Webanwendung der Beispiel-Web-App veranschaulicht, wie ASP.NET MVC 4-Anwendungen mithilfe der Entity Framework 5 Code First und Visual Studio 2012 erstellt werden. Informationen zu dieser Tutorialreihe finden Sie im [ersten Tutorial der Reihe](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Sie können die tutorialreihe von Anfang an starten oder [ein Starter-Projekt für dieses Kapitel herunterladen](building-the-ef5-mvc4-chapter-downloads.md) und hier beginnen.
> 
> > [!NOTE] 
> > 
> > Wenn Sie auf ein Problem stoßen, das Sie nicht beheben können, [Laden Sie das abgeschlossene Kapitel herunter](building-the-ef5-mvc4-chapter-downloads.md) , und versuchen Sie, das Problem zu reproduzieren. Im Allgemeinen können Sie die Lösung für das Problem finden, indem Sie Ihren Code mit dem abgeschlossenen Code vergleichen. Informationen zu häufigen Fehlern und deren Lösung finden Sie unter [Fehler und](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors) Problem Umgehungen.

In den vorherigen Tutorials haben Sie mit einem einfachen Datenmodell gearbeitet, das aus drei Entitäten besteht. In diesem Tutorial fügen Sie weitere Entitäten und Beziehungen hinzu und passen das Datenmodell an, indem Sie Formatierungs-, Validierungs-und Daten Bank Zuordnungsregeln angeben. Es werden zwei Möglichkeiten zum Anpassen des Datenmodells angezeigt: durch Hinzufügen von Attributen zu Entitäts Klassen und durch Hinzufügen von Code zur Daten Bank Kontext Klasse.

Wenn Sie dies erledigt haben, bilden die Entitätsklassen ein vollständiges Datenmodell, das in der folgenden Abbildung dargestellt wird:

![School_class_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image1.png)

## <a name="customize-the-data-model-by-using-attributes"></a>Anpassen des Datenmodells mithilfe von Attributen

In diesem Abschnitt wird das Anpassen des Datenmodells mithilfe von Attributen erläutert, die Regeln zur Formatierung, Validierung und Datenbankzuordnung angeben. In einigen der folgenden Abschnitte erstellen Sie das gesamte `School` Datenmodell, indem Sie den bereits erstellten Klassenattribute hinzufügen und neue Klassen für die verbleibenden Entitäts Typen im Modell erstellen.

### <a name="the-datatype-attribute"></a>Das DataType-Attribut

Für die Anmeldedaten von Studenten zeigen alle Webseiten derzeit die Zeit und das Datum an, obwohl für dieses Feld nur das Datum relevant ist. Indem Sie Attribute für die Datenanmerkung verwenden, können Sie eine Codeänderungen vornehmen, durch die das Anzeigeformat in jeder Ansicht korrigiert wird, in der die Daten angezeigt werden. Sie können dies beispielhaft testen, indem Sie ein Attribut zur `EnrollmentDate`-Eigenschaft der `Student`-Klasse hinzufügen.

Fügen Sie in *models\student.cs*eine `using`-Anweisung für den `System.ComponentModel.DataAnnotations`-Namespace hinzu, und fügen Sie der `EnrollmentDate`-Eigenschaft `DataType` und `DisplayFormat` Attribute hinzu, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,13-14)]

Das [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) -Attribut wird verwendet, um einen Datentyp anzugeben, der spezifischer als der Daten Bank interne Typ ist. In diesem Fall soll nur das Datum verfolgt werden, nicht das Datum und die Zeit. Die [DataType-Enumeration](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) stellt viele Datentypen bereit, wie z. b. *Datum, Uhrzeit, PhoneNumber, Currency, EmailAddress* usw. Das `DataType`-Attribut kann der Anwendung auch ermöglichen, typspezifische Features bereitzustellen. Beispielsweise kann ein `mailto:` Link für [DataType. EmailAddress](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)erstellt werden, und für [DataType. Date](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) in Browsern, die [HTML5](http://html5.org/)unterstützen, kann eine Datumsauswahl angegeben werden. Die [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) -Attribute gibt HTML 5-Attribute aus, die von HTML 5 [-](http://ejohn.org/blog/html-5-data-attributes/) Browsern verstanden werden können. Die [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) -Attribute bieten keine Validierung.

`DataType.Date` gibt nicht das Format des Datums an, das angezeigt wird. Standardmäßig wird das Datenfeld gemäß den Standardformaten basierend auf der [CultureInfo](https://msdn.microsoft.com/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx)des Servers angezeigt.

Das `DisplayFormat`-Attribut dient zum expliziten Angeben des Datumsformats:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample2.cs)]

Die `ApplyFormatInEditMode` Einstellung gibt an, dass die angegebene Formatierung auch angewendet werden soll, wenn der Wert zur Bearbeitung in einem Textfeld angezeigt wird. (Möglicherweise möchten Sie das Währungssymbol für einige Felder nicht ändern – z. b. für Währungswerte ist es möglicherweise nicht erforderlich, dass das Währungssymbol im Textfeld bearbeitet wird.)

Sie können das [Display Format](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) -Attribut eigenständig verwenden, aber es ist in der Regel eine gute Idee, auch das [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) -Attribut zu verwenden. Das `DataType`-Attribut übermittelt die *Semantik* der Daten im Gegensatz zum Rendering auf einem Bildschirm und bietet die folgenden Vorteile, die Sie mit `DisplayFormat`nicht erhalten:

- Der Browser kann HTML5-Features aktivieren (z. b. zum Anzeigen eines Kalender Steuer Elements, des Gebiets Schema-entsprechenden Währungs Symbols, von e-Mail-Links usw.).
- Standardmäßig wird der Browserdaten basierend [auf Ihrem Gebiets](https://msdn.microsoft.com/library/vstudio/wyzd2bce.aspx)Schema im richtigen Format Rendering.
- Das [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) -Attribut kann MVC ermöglichen, die richtige Feld Vorlage zum renderingder Daten auszuwählen ( [Display Format](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) , wenn es eigenständig verwendet wird, verwendet die Zeichen folgen Vorlage). Weitere Informationen finden Sie in den Vorlagen von Brad Wilson [ASP.NET MVC 2](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). (Obwohl für MVC 2 geschrieben, gilt dieser Artikel weiterhin für die aktuelle Version von ASP.NET MVC.)

Wenn Sie das `DataType`-Attribut mit einem Datumsfeld verwenden, müssen Sie auch das `DisplayFormat`-Attribut angeben, um sicherzustellen, dass das Feld ordnungsgemäß in Chrome-Browsern gerendert wird. Weitere Informationen finden Sie in [diesem StackOverflow-Thread](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie).

Führen Sie die Index Seite "Student" erneut aus, und beachten Sie, dass die Zeiten für die Registrierungsdaten nicht mehr angezeigt werden. Das gleiche gilt für jede Ansicht, die das `Student` Modell verwendet.

![Students_index_page_with_formatted_date](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image2.png)

### <a name="the-stringlengthattribute"></a>Das StringLengthAttribute

Mithilfe von Attributen können Sie auch Daten Validierungsregeln und-Nachrichten angeben. Angenommen, Sie möchten sicherstellen, dass Benutzer nicht mehr als 50 Zeichen für einen Namen eingeben. Fügen Sie zum Hinzufügen dieser Einschränkung den Eigenschaften `LastName` und `FirstMidName` [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) -Attribute hinzu, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=10,12)]

Das [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) -Attribut verhindert nicht, dass ein Benutzer Leerraum für einen Namen eingibt. Mit dem [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) -Attribut können Sie Einschränkungen auf die Eingabe anwenden. Der folgende Code erfordert beispielsweise, dass das erste Zeichen in Großbuchstaben und die restlichen Zeichen alphabetisch sind:

`[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]`

Das Attribut " [MaxLength](https://msdn.microsoft.com/library/System.ComponentModel.DataAnnotations.MaxLengthAttribute.aspx) " stellt eine ähnliche Funktionalität wie das [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) -Attribut bereit, stellt jedoch keine Client seitige Validierung bereit.

Führen Sie die Anwendung aus, und klicken Sie auf die Registerkarte **Studenten** Sie erhalten die folgende Fehlermeldung:

*Das Modell, das den "schoolContext"-Kontext unterstützt, wurde seit der Erstellung der Datenbank geändert. Verwenden Sie Code First-Migrationen, um die Datenbank zu aktualisieren ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269)).*

Das Datenbankmodell hat sich auf eine Weise geändert, die eine Änderung des Datenbankschemas erfordert, und Entity Framework erkannte. Sie verwenden Migrationen, um das Schema zu aktualisieren, ohne Daten zu verlieren, die Sie der Datenbank mithilfe der Benutzeroberfläche hinzugefügt haben. Wenn Sie Daten geändert haben, die von der `Seed`-Methode erstellt wurden, werden diese aufgrund der [addorupdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx) -Methode, die Sie in der `Seed`-Methode verwenden, wieder in ihren ursprünglichen Zustand geändert. ([Addorupdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx) entspricht einem Upsert-Vorgang aus der Daten Bank Terminologie.)

Geben Sie die folgenden Befehle in die Paket-Manager-Konsole ein:

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample4.cmd)]

Mit dem `add-migration MaxLengthOnNames`-Befehl wird eine Datei mit dem Namen *&lt;Timestamp&gt;\_MaxLengthOnNames.cs*erstellt. Diese Datei enthält Code, mit dem die Datenbank so aktualisiert wird, dass Sie dem aktuellen Datenmodell entspricht. Der Zeitstempel, der dem Migrations Dateinamen vorangestellt wird, wird von Entity Framework verwendet, um die Migrationen zu sortieren. Wenn Sie mehrere Migrationen erstellt haben, wenn Sie die Datenbank löschen oder das Projekt mithilfe von Migrationen bereitstellen, werden alle Migrationen in der Reihenfolge angewendet, in der Sie erstellt wurden.

Führen Sie die Seite **Erstellen** aus, und geben Sie einen Namen mit mehr als 50 Zeichen ein. Sobald Sie 50 Zeichen überschreiten, zeigt die Client seitige Validierung sofort eine Fehlermeldung an.

![Client seitiges Val-Fehler](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image3.png)

### <a name="the-column-attribute"></a>Das Column-Attribut

Sie können ebenfalls Attribute verwenden, um zu steuern, wie Ihre Klassen und Eigenschaften der Datenbank zugeordnet werden. Angenommen, Sie haben den Namen `FirstMidName` für das Feld „first-name“ verwendet, da das Feld ebenfalls einen Zweitnamen enthalten kann. Sie möchten jedoch, dass die Datenbankspalte in `FirstName` umbenannt wird, damit Benutzer, die Ad-hob-Abfragen für die Datenbank schreiben, an diesen Namen gewöhnt sind. Sie können das `Column`-Attribut verwenden, um diese Zuordnung durchzuführen.

Das `Column`-Attribut gibt an, dass bei der Erstellung der Datenbank die Spalte des `Student`-Elements, die der `FirstMidName`-Eigenschaft zugeordnet ist, den Namen `FirstName` erhält. Wenn Ihr Code also auf `Student.FirstMidName` verweist, stammen die Daten aus der `FirstName`-Spalte der `Student`-Tabelle oder werden in dieser aktualisiert. Wenn Sie keine Spaltennamen angeben, erhalten Sie denselben Namen wie der Eigenschaftsname.

Fügen Sie der `FirstMidName`-Eigenschaft eine using-Anweisung für [System. ComponentModel. DataAnnotations. Schema](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.aspx) und das Column Name-Attribut hinzu, wie im folgenden hervorgehobenen Code gezeigt:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample5.cs?highlight=4,14)]

Durch das Hinzufügen des [Column-Attributs](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx) wird das Modell geändert, das den schoolContext sichert, sodass es nicht mit der Datenbank identisch ist. Geben Sie die folgenden Befehle in der PMC ein, um eine weitere Migration zu erstellen:

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample6.cmd)]

Doppelklicken Sie in **Server-Explorer** (**Datenbank-Explorer** bei Verwendung von Express für Web) auf die Tabelle *Student* .

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image4.png)

Die folgende Abbildung zeigt den ursprünglichen Spaltennamen wie vor dem Anwenden der ersten beiden Migrationen. Neben dem Spaltennamen, der von `FirstMidName` in `FirstName`geändert wird, haben sich die beiden namens Spalten von `MAX` Länge in 50 Zeichen geändert.

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image5.png)

Sie können auch Änderungen an der Daten Bank Zuordnung mithilfe der [flüssigen API](https://msdn.microsoft.com/data/jj591617)vornehmen, wie Sie später in diesem Tutorial sehen werden.

> [!NOTE]
> Wenn Sie versuchen, eine Kompilierung durchführen, bevor Sie alle diese Entitäts Klassen erstellt haben, erhalten Sie möglicherweise Compilerfehler.

## <a name="create-the-instructor-entity"></a>Erstellen der Instructor-Entität

![Instructor_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image6.png)

Erstellen Sie *models\instructor .cs*, und ersetzen Sie den Vorlagen Code durch den folgenden Code:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample7.cs)]

Beachten Sie, dass einige Eigenschaften in den Entitäten `Student` und `Instructor` identisch sind. Im Lernprogramm zum [Implementieren von Vererbung](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md) werden Sie später in dieser Serie mithilfe der Vererbung umgestalten, um diese Redundanz auszuschließen.

### <a name="the-required-and-display-attributes"></a>Erforderliche Attribute und Anzeige Attribute

Die Attribute für die `LastName`-Eigenschaft geben an, dass es sich um ein erforderliches Feld handelt, dass die Beschriftung für das Textfeld "Nachname" lauten soll (anstelle des Eigenschafts namens, bei dem es sich um einen "LastName" ohne Leerzeichen handelt) und dass der Wert nicht länger als 50 Zeichen sein darf.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample8.cs)]

Das [StringLength-Attribut](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) legt die maximale Länge in der Datenbank fest und bietet Client seitige und serverseitige Validierung für ASP.NET MVC. Sie können ebenfalls die mindestens erforderliche Zeichenfolgenlänge in diesem Attribut angeben, dieser Wert hat jedoch keine Auswirkung auf das Datenbankschema. Das [erforderliche Attribut](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) ist für Werttypen wie DateTime, int, Double und float nicht erforderlich. Werttypen kann kein NULL-Wert zugewiesen werden, daher sind Sie von Natur aus erforderlich. Sie können das [erforderliche Attribut](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) entfernen und es durch einen Parameter für die minimale Länge für das `StringLength` Attribut ersetzen:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample9.cs?highlight=2)]

Sie können mehrere Attribute in einer Zeile platzieren, sodass Sie auch die Klasse Instructor wie folgt schreiben können:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

### <a name="the-fullname-calculated-property"></a>Die berechnete FullName-Eigenschaft.

Bei `FullName` handelt es sich um eine berechnete Eigenschaft, die einen Wert zurückgibt, der durch das Verketten von zwei weiteren Eigenschaften erstellt wird. Daher verfügt sie nur über einen `get` Accessor, und in der Datenbank wird keine `FullName` Spalte generiert.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

### <a name="the-courses-and-officeassignment-navigation-properties"></a>Die Navigations Eigenschaften "Kurse" und "officezuweisung"

Bei den Eigenschaften `Courses` und `OfficeAssignment` handelt es sich um Navigationseigenschaften. Wie bereits erwähnt, sind Sie in der Regel als [virtuell](https://msdn.microsoft.com/library/9fkccyh4(v=vs.110).aspx) definiert, sodass Sie eine Entity Framework Funktion mit dem Namen [Lazy Loading](https://msdn.microsoft.com/magazine/hh205756.aspx)nutzen können. Wenn eine Navigations Eigenschaft mehrere Entitäten enthalten kann, muss der Typ außerdem die [ICollection&lt;t&gt;](https://msdn.microsoft.com/library/92t2ye13.aspx) -Schnittstelle implementieren. (Beispiel: [IList&lt;t&gt;](https://msdn.microsoft.com/library/5y536ey6.aspx) qualifiziert, aber nicht [IEnumerable&lt;t&gt;](https://msdn.microsoft.com/library/9eekhta0.aspx) , weil `IEnumerable<T>` [Add](https://msdn.microsoft.com/library/63ywd54z.aspx)nicht implementiert.

Ein Dozent kann eine beliebige Anzahl von Kursen unterrichten, sodass `Courses` als eine Auflistung von `Course` Entitäten definiert ist. Unser Geschäftsregel Status ein Dozent kann nur über höchstens ein Büro verfügen, sodass `OfficeAssignment` als einzelne `OfficeAssignment` Entität definiert ist (was `null` werden kann, wenn kein Büro zugewiesen ist).

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

## <a name="create-the-officeassignment-entity"></a>Erstellen der officezuweisungs-Entität

![OfficeAssignment_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image7.png)

Erstellen Sie *models\officezuordment.cs* mit dem folgenden Code:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

Erstellen Sie das Projekt, das Ihre Änderungen speichert und überprüft, ob Sie keine Kopier-und Einfüge Fehler vorgenommen haben, die der Compiler erfassen kann.

### <a name="the-key-attribute"></a>Das Schlüssel Attribut

Zwischen den `Instructor` und den `OfficeAssignment` Entitäten besteht eine 1:0-oder 1-Beziehung. Eine Büro Zuweisung ist nur in Bezug auf den Dozenten vorhanden, dem Sie zugewiesen ist. Deshalb ist der Primärschlüssel auch der Fremdschlüssel für die `Instructor` Entität. Allerdings kann der Entity Framework `InstructorID` nicht automatisch als Primärschlüssel dieser Entität erkennen, weil der Name nicht der `ID`-oder *className* -`ID` Benennungs Konvention folgt. Deshalb wird das `Key`-Attribut verwendet, um „InstructorID“ als Schlüssel zu erkennen:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample14.cs)]

Sie können auch das `Key`-Attribut verwenden, wenn die Entität über einen eigenen Primärschlüssel verfügt, Sie aber eine andere Eigenschaft als `classnameID` oder `ID`benennen möchten. Standardmäßig behandelt EF den Schlüssel als nicht-Daten Bank generiert, da die Spalte für eine identifizierende Beziehung vorgesehen ist.

### <a name="the-foreignkey-attribute"></a>Das Fremdschlüssel Attribut.

Wenn eine 1: NULL-oder eine 1-zu-eins-Beziehung zwischen zwei Entitäten vorliegt (z. b. zwischen `OfficeAssignment` und `Instructor`), kann EF nicht herausfinden, welches Ende der Beziehung der Prinzipal ist und welches Ende abhängig ist. Eine-zu-eins-Beziehung hat in jeder Klasse eine Verweis Navigations Eigenschaft für die andere Klasse. Das fremd [Schlüssel Attribut](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx) kann auf die abhängige Klasse angewendet werden, um die Beziehung herzustellen. Wenn Sie das fremd [Schlüssel Attribut](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx)weglassen, erhalten Sie die folgende Fehlermeldung, wenn Sie versuchen, die Migration zu erstellen:

Das Prinzipal Ende einer Zuordnung zwischen den Typen "contosouniversity. Models. officezuweisung" und "contosouniversity. Models. Instructor" kann nicht bestimmt werden. Das Prinzipal Ende dieser Zuordnung muss entweder mithilfe der vertrauenden API der Beziehung oder mit Daten Anmerkungen explizit konfiguriert werden.

Später in diesem Tutorial zeigen wir, wie Sie diese Beziehung mit der fließend-API konfigurieren.

### <a name="the-instructor-navigation-property"></a>Die Instructor-Navigations Eigenschaft

Die `Instructor`-Entität verfügt über eine `OfficeAssignment` Navigations Eigenschaft, die NULL-Werte zulässt (weil ein Dozenten möglicherweise keine Office `InstructorID`-Zuweisung hat), und die `OfficeAssignment`-Entität verfügt über eine `Instructor` Navigations Eigenschaft, die nicht auf NULL festgelegt werden kann (da eine Office-Zuweisung ohne einen Dozenten nicht vorhanden sein kann). Wenn eine `Instructor` Entität über eine verknüpfte `OfficeAssignment` Entität verfügt, verfügt jede Entität in der zugehörigen Navigations Eigenschaft über einen Verweis auf die andere Entität.

Sie können ein `[Required]`-Attribut in der Instructor-Navigations Eigenschaft ablegen, um anzugeben, dass es sich um einen zugehörigen Dozenten handeln muss, aber Sie müssen dies nicht tun, da der InstructorID-Fremdschlüssel (der auch der Schlüssel für diese Tabelle ist) keine NULL-Werte zulässt.

## <a name="modify-the-course-entity"></a>Ändern der Entität „Course“

![Course_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image8.png)

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

## <a name="creating-the-department-entity"></a>Erstellen der Entität "Department"

![Department_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image9.png)

Erstellen Sie *models\department.cs* mit dem folgenden Code:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample20.cs)]

### <a name="the-column-attribute"></a>Das Column-Attribut

Zuvor haben Sie das [Column-Attribut](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx) verwendet, um die Zuordnung von Spaltennamen zu ändern. Im Code für die `Department`-Entität wird das `Column`-Attribut verwendet, um die SQL-Datentyp Zuordnung zu ändern, sodass die Spalte mit dem SQL Server [Money](https://msdn.microsoft.com/library/ms179882.aspx) -Typ in der Datenbank definiert wird:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample21.cs)]

Die Spalten Zuordnung ist im Allgemeinen nicht erforderlich, da die Entity Framework in der Regel den entsprechenden SQL Server Datentyp basierend auf dem CLR-Typ auswählt, den Sie für die Eigenschaft definieren. Der CLR-Typ `decimal` wird dem SQL Server-Typ `decimal` zugeordnet. In diesem Fall wissen Sie jedoch, dass die Spalte Währungs Beträge enthält, und der [Money](https://msdn.microsoft.com/library/ms179882.aspx) -Datentyp ist besser geeignet.

### <a name="foreign-key-and-navigation-properties"></a>Fremdschlüssel-und Navigations Eigenschaften

Die Fremdschlüssel- und Navigationseigenschaften stellen folgende Beziehungen dar:

- Eine Abteilung kann einen Administrator besitzen oder nicht, und bei einem Administrator handelt es sich immer um einen Dozenten. Daher ist die `InstructorID`-Eigenschaft als Fremdschlüssel für die `Instructor` Entität enthalten, und ein Fragezeichen wird nach der `int` Type-Bezeichnung hinzugefügt, um die Eigenschaft als Nullable zu markieren. Die Navigations Eigenschaft hat den Namen `Administrator`, enthält jedoch eine `Instructor` Entität: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample22.cs)]
- Eine Abteilung kann viele Kurse haben, sodass es eine `Courses` Navigations Eigenschaft gibt: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample23.cs)]

  > [!NOTE]
  > Gemäß der Konvention aktiviert Entity Framework das kaskadierende Delete für nicht auf NULL festlegbare Fremdschlüssel und für m:n-Beziehungen. Dies kann zu zirkulären kaskadierenden Löschregeln führen, die bei der Ausführung des Initialisierungs Codes eine Ausnahme auslösen. Wenn Sie die `Department.InstructorID`-Eigenschaft beispielsweise nicht als Nullable definiert haben, erhalten Sie die folgende Ausnahme Meldung, wenn der Initialisierer ausgeführt wird: "die referenzielle Beziehung führt zu einem zyklischen Verweis, der nicht zulässig ist." Wenn Ihre Geschäftsregeln `InstructorID` Eigenschaft als nicht auf NULL festleg Bare Eigenschaft erforderlich sind, müssten Sie die folgende fließende API verwenden, um die Lösch Weitergabe für die Beziehung zu deaktivieren: 

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample24.cs)]

## <a name="modifying-the-student-entity"></a>Ändern der Entität "Student"

![Student_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image10.png)

Ersetzen Sie in *models\student.cs*den Code, den Sie zuvor hinzugefügt haben, durch den folgenden Code. Die Änderungen werden hervorgehoben.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample25.cs?highlight=12,15,24-27)]

## <a name="the-enrollment-entity"></a>Die Entität "Registrierung"

 Ersetzen Sie in *models\registriment.cs*den Code, den Sie zuvor hinzugefügt haben, durch den folgenden Code:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample26.cs?highlight=16)]

### <a name="foreign-key-and-navigation-properties"></a>Fremdschlüssel-und Navigations Eigenschaften

Die Fremdschlüssel- und Navigationseigenschaften stellen folgende Beziehungen dar:

- Ein Anmeldungsdatensatz gilt für einen einzelnen Kurs, sodass es eine `CourseID`-Fremdschlüsseleigenschaft und eine `Course`-Navigationseigenschaft gibt: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample27.cs)]
- Ein Anmeldungsdatensatz gilt für einen einzelnen Studenten, sodass es eine `StudentID`-Fremdschlüsseleigenschaft und eine `Student`-Navigationseigenschaft gibt: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample28.cs)]

### <a name="many-to-many-relationships"></a>n:n-Beziehungen

Es gibt eine m:n-Beziehung zwischen den Entitäten `Student` und `Course`, und die `Enrollment` Entität fungiert als m:n-jointabelle *mit Nutzlast* in der Datenbank. Dies bedeutet, dass die `Enrollment` Tabelle zusätzliche Daten neben Fremdschlüsseln für die verbundenen Tabellen enthält (in diesem Fall ein Primärschlüssel und eine `Grade`-Eigenschaft).

Die folgende Abbildung stellt dar, wie diese Beziehungen in einem Entitätsdiagramm aussehen. (Dieses Diagramm wurde mithilfe der [Entity Framework Power Tools](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d)generiert; das Erstellen des Diagramms ist nicht Teil des Tutorials, sondern wird hier nur als Abbildung verwendet.)

![Student-Course_many-many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image11.png)

Jede Beziehungs Linie hat ein 1 an einem Ende und ein Sternchen (\*) am anderen, das eine 1: n-Beziehung angibt.

Wenn die `Enrollment` Tabelle keine Informationen zur Qualität enthielt, muss Sie nur die beiden Fremdschlüssel `CourseID` und `StudentID`enthalten. In diesem Fall würde es sich um eine m:n-jointabelle *ohne Nutzlast* (oder eine *reine jointabelle*) in der Datenbank beziehen, und Sie müssten überhaupt keine Modell Klasse erstellen. Die Entitäten `Instructor` und `Course` haben diese Art von m:n-Beziehung, und wie Sie sehen können, gibt es keine Entitäts Klasse zwischen Ihnen:

![Instructor-Course_many-many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image12.png)

In der Datenbank ist jedoch eine jointabelle erforderlich, wie im folgenden Daten Bank Diagramm gezeigt:

![Instructor-Course_many-many_relationship_tables](https://asp.net/media/2577802/Windows-Live-Writer_Creating-a.NET-MVC-Application-4-of-10h1_B662_Instructor-Course_many-to-many_relationship_tables_03e042cf-db89-4b4c-985a-e458351ada76.png)

Der Entity Framework erstellt automatisch die `CourseInstructor` Tabelle, und Sie lesen und aktualisieren diese indirekt, indem Sie die `Instructor.Courses` und `Course.Instructors` Navigations Eigenschaften lesen und aktualisieren.

## <a name="entity-diagram-showing-relationships"></a>Entitätsdiagramm mit angezeigten Beziehungen

Die folgende Abbildung stellt das Diagramm dar, das von Entity Framework Power Tools für das vollständige Modell „School“ erstellt wird.

![School_data_model_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image13.png)

Neben den m:n-Beziehungslinien (\* \*) und den 1: n-Beziehungs Linien (1 bis \*) Sie sehen hier die 1: NULL-oder eine Beziehungs Linie (1 bis 0.. 1) zwischen den Entitäten "`Instructor`" und "`OfficeAssignment`" und der NULL-oder 1: n-Beziehungs Linie (0.. 1 bis \*) zwischen den Entitäten "Instructor" und "Department".

## <a name="customize-the-data-model-by-adding-code-to-the-database-context"></a>Anpassen des Datenmodells durch Hinzufügen von Code zum Daten Bank Kontext

Als Nächstes fügen Sie der `SchoolContext`-Klasse die neuen Entitäten hinzu und passen einige der zuder Zuordnung mithilfe von [fließenden API](https://msdn.microsoft.com/data/jj591617) -Aufrufen an. (Die API ist "fließend", da Sie häufig verwendet wird, um eine Reihe von Methoden aufrufen in einer einzigen Anweisung zu überspannen.)

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

## <a name="seed-the-database-with-test-data"></a>Ausführen eines Seedings für die Datenbank mit Testdaten

Ersetzen Sie den Code in der Datei *migrations\configuration.cs* durch den folgenden Code, um Seed-Daten für die neuen Entitäten bereitzustellen, die Sie erstellt haben.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample32.cs)]

Wie Sie im ersten Tutorial gesehen haben, aktualisiert der größte Teil dieses Codes einfach neue Entitäts Objekte und lädt Beispiel Daten in Eigenschaften, die für den Test erforderlich sind. Beachten Sie jedoch, dass die `Course`-Entität, die über eine m:n-Beziehung mit der `Instructor`-Entität verfügt, behandelt wird:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample33.cs)]

Wenn Sie ein `Course` Objekt erstellen, initialisieren Sie die `Instructors` Navigations Eigenschaft als leere Auflistung, indem Sie den Code-`Instructors = new List<Instructor>()`verwenden. Dies ermöglicht es, mithilfe der `Instructors.Add`-Methode `Instructor` Entitäten hinzuzufügen, die mit diesem `Course` verknüpft sind. Wenn Sie keine leere Liste erstellt haben, können Sie diese Beziehungen nicht hinzufügen, da die `Instructors`-Eigenschaft NULL wäre und keine `Add`-Methode haben würde. Sie können auch die Listen Initialisierung dem Konstruktor hinzufügen.

## <a name="add-a-migration-and-update-the-database"></a>Hinzufügen einer Migration und Aktualisieren der Datenbank

Geben Sie in der PMC den `add-migration`-Befehl ein:

`PM> add-Migration Chap4`

Wenn Sie versuchen, die Datenbank zu diesem Zeitpunkt zu aktualisieren, erhalten Sie die folgende Fehlermeldung:

*Die ALTER TABLE-Anweisung steht in Konflikt mit der FOREIGN KEY-Einschränkung "FK\_dbo. Kurs\_dbo. Abteilung\_DepartmentID ". Der Konflikt trat in der Datenbank "Conto souniversity", Tabelle "dbo" auf. Department ", Spalte" DepartmentID ".*

Bearbeiten Sie den &lt;*Zeitstempel-&gt;\_Chap4.cs* -Datei, und nehmen Sie die folgenden Codeänderungen vor (Sie fügen eine SQL-Anweisung hinzu und ändern eine `AddColumn`-Anweisung):

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample34.cs?highlight=14-18)]

(Stellen Sie sicher, dass Sie die vorhandene `AddColumn` Zeile auskommentieren oder löschen, wenn Sie die neue Zeile hinzufügen, oder Sie erhalten eine Fehlermeldung, wenn Sie den `update-database`-Befehl eingeben.)

Manchmal müssen Sie beim Ausführen von Migrationen mit vorhandenen Daten Stub-Daten in die Datenbank einfügen, um Foreign Key-Einschränkungen zu erfüllen, und das ist das, was Sie jetzt tun. Der generierte Code fügt der `Course` Tabelle einen `DepartmentID` Fremdschlüssel hinzu, der keine NULL-Werte zulässt. Wenn in der `Course` Tabelle bereits Zeilen vorhanden sind, wenn der Code ausgeführt wird, misslingt der `AddColumn` Vorgang, da SQL Server nicht weiß, welcher Wert in die Spalte eingefügt werden darf, die nicht NULL sein darf. Daher haben Sie den Code so geändert, dass die neue Spalte einen Standardwert erhält, und Sie haben eine Stub-Abteilung mit dem Namen "Temp" erstellt, die als Standard Abteilung fungiert. Wenn `Course` Zeilen vorhanden sind, wenn dieser Code ausgeführt wird, sind Sie folglich alle mit der Abteilung "Temp" verknüpft.

Wenn die `Seed`-Methode ausgeführt wird, werden Zeilen in die `Department` Tabelle eingefügt, und vorhandene `Course` Zeilen werden mit den neuen `Department` Zeilen verknüpft. Wenn Sie keine Kurse in der Benutzeroberfläche hinzugefügt haben, benötigen Sie die "Temp"-Abteilung oder den Standardwert für die `Course.DepartmentID` Spalte nicht mehr. Um die Möglichkeit zuzulassen, dass ein Benutzer möglicherweise Kurse mithilfe der Anwendung hinzugefügt hat, möchten Sie auch den Code der `Seed`-Methode aktualisieren, um sicherzustellen, dass alle `Course` Zeilen (nicht nur die, die von früheren Ausführungen der `Seed`-Methode eingefügt wurden) gültige `DepartmentID` Werte aufweisen, bevor Sie den Standardwert aus der Spalte entfernen und die Abteilung "Temp" löschen.

Nachdem Sie die &lt;*Zeitstempel-&gt;\_Chap4.cs* -Datei bearbeitet haben, geben Sie den `update-database` Befehl in der PMC ein, um die Migration auszuführen.

> [!NOTE]
> Beim Migrieren von Daten und vornehmen von Schema Änderungen können andere Fehler auftreten. Wenn Sie Migrations Fehler erhalten, die Sie nicht beheben können, können Sie entweder die Verbindungs Zeichenfolge in der Datei " *Web. config* " ändern oder die Datenbank löschen. Der einfachste Ansatz besteht darin, die Datenbank in der Datei " *Web. config* " umzubenennen. Ändern Sie z. b. den Datenbanknamen in Cu\_Test, wie im folgenden Beispiel gezeigt:
> 
> [!code-xml[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample35.xml?highlight=1-2)]
> 
> Bei einer neuen Datenbank gibt es keine zu migrierenden Daten, und der `update-database`-Befehl wird wahrscheinlich ohne Fehler ausgeführt. Anweisungen zum Löschen der Datenbank finden Sie unter Vorgehensweise beim Löschen [einer Datenbank aus Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).

Öffnen Sie die Datenbank in **Server-Explorer** wie zuvor, und erweitern Sie den Knoten **Tabellen** , um zu sehen, dass alle Tabellen erstellt wurden. (Klicken Sie auf die Schaltfläche **Aktualisieren** , wenn Sie noch **Server-Explorer** bereits geöffnet haben.)

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image14.png)

Sie haben keine Modell Klasse für die `CourseInstructor` Tabelle erstellt. Wie bereits erwähnt, handelt es sich hierbei um eine jointabelle für die m:n-Beziehung zwischen den Entitäten `Instructor` und `Course`.

Klicken Sie mit der rechten Maustaste auf die Tabelle `CourseInstructor`, und wählen Sie **Tabellendaten anzeigen** aus, um zu überprüfen, ob diese Daten als Ergebnis der `Instructor` Entitäten enthalten, die Sie der `Course.Instructors` Navigations Eigenschaft hinzugefügt haben.

![Table_data_in_CourseInstructor_table](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image15.png)

## <a name="summary"></a>Summary

Sie besitzen nun ein komplexeres Datenmodell und eine zugehörige Datenbank. Im folgenden Tutorial erfahren Sie mehr über die verschiedenen Möglichkeiten, auf verwandte Daten zuzugreifen.

Links zu anderen Entity Framework Ressourcen finden Sie in der [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Zurück](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Weiter](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
