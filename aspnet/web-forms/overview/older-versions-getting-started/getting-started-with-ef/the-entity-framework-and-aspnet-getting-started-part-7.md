---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
title: Einführung in Entity Framework 4,0 Database First und ASP.NET 4 Web Forms-Part 7 | Microsoft-Dokumentation
author: tdykstra
description: Die Beispiel-Webanwendung der Beispiel-Web-App veranschaulicht, wie Sie mithilfe der Entity Framework Web Forms Anwendungen erstellen. Die Beispielanwendung ist...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: f8afb245-b705-419c-8790-0b295e90d5e2
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
msc.type: authoredcontent
ms.openlocfilehash: 18d4b44c5e23fd6942c3adf48a33a5602e6df6d0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78488523"
---
# <a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-7"></a>Die ersten Schritte mit Entity Framework 4,0 Database First und ASP.NET 4 Web Forms Teil 7

von [Tom Dykstra](https://github.com/tdykstra)

> Die Beispiel-Webanwendung der Beispiel-Web-App veranschaulicht, wie Sie ASP.net-Web Forms Anwendungen mithilfe von Entity Framework 4,0 und Visual Studio 2010 erstellen. Weitere Informationen zur tutorialreihe finden Sie [im ersten Tutorial der Reihe](the-entity-framework-and-aspnet-getting-started-part-1.md) .

## <a name="using-stored-procedures"></a>Verwenden von gespeicherten Prozeduren

Im vorherigen Tutorial haben Sie ein "Tabelle pro Hierarchie"-Vererbungsmuster implementiert. In diesem Tutorial erfahren Sie, wie Sie gespeicherte Prozeduren verwenden, um mehr Kontrolle über den Datenbankzugriff zu erhalten.

Mit der Entity Framework können Sie angeben, dass gespeicherte Prozeduren für den Datenbankzugriff verwendet werden sollen. Für jeden Entitätstyp können Sie eine gespeicherte Prozedur angeben, die zum Erstellen, aktualisieren oder Löschen von Entitäten dieses Typs verwendet werden soll. Im Datenmodell können Sie dann Verweise auf gespeicherte Prozeduren hinzufügen, die Sie zum Ausführen von Aufgaben wie dem Abrufen von Entitätenmengen verwenden können.

Die Verwendung gespeicherter Prozeduren ist eine gängige Anforderung für den Datenbankzugriff In einigen Fällen kann es erforderlich sein, dass der gesamte Datenbankzugriff durch gespeicherte Prozeduren aus Sicherheitsgründen durchlaufen wird. In anderen Fällen empfiehlt es sich, Geschäftslogik in einige der Prozesse zu erstellen, die vom Entity Framework beim Aktualisieren der Datenbank verwendet werden. Wenn z. b. eine Entität gelöscht wird, können Sie Sie in eine Archivdatenbank kopieren. Oder wenn eine Zeile aktualisiert wird, können Sie eine Zeile in eine Protokollierungs Tabelle schreiben, in der aufgezeichnet wird, wer die Änderung vorgenommen hat. Sie können diese Arten von Aufgaben in einer gespeicherten Prozedur ausführen, die aufgerufen wird, wenn der Entity Framework eine Entität löscht oder eine Entität aktualisiert.

Wie im vorherigen Tutorial werden Sie keine neuen Seiten erstellen. Stattdessen ändern Sie die Art und Weise, in der der Entity Framework auf die Datenbank für einige der Seiten zugreift, die Sie bereits erstellt haben.

In diesem Tutorial erstellen Sie in der-Datenbank gespeicherte Prozeduren zum Einfügen von `Student` und `Instructor` Entitäten. Sie werden dem Datenmodell hinzugefügt, und Sie geben an, dass der Entity Framework diese zum Hinzufügen von `Student` und `Instructor` Entitäten zur Datenbank verwenden soll. Außerdem erstellen Sie eine gespeicherte Prozedur, die Sie zum Abrufen von `Course` Entitäten verwenden können.

## <a name="creating-stored-procedures-in-the-database"></a>Erstellen von gespeicherten Prozeduren in der Datenbank

(Wenn Sie die Datei " *School. mdf* " aus dem Projekt verwenden, das in diesem Tutorial zum Herunterladen verfügbar ist, können Sie diesen Abschnitt überspringen, da die gespeicherten Prozeduren bereits vorhanden sind.)

Erweitern Sie in **Server-Explorer**den Eintrag *School. mdf*, klicken Sie mit der rechten Maustaste auf **gespeicherte Prozeduren**, und wählen Sie dann **neue gespeicherte Prozedur**

[![image15](the-entity-framework-and-aspnet-getting-started-part-7/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image1.png)

Kopieren Sie die folgenden SQL-Anweisungen, und fügen Sie Sie in das Fenster gespeicherte Prozedur ein. ersetzen Sie dabei die gespeicherte Prozedur Skeleton.

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample1.sql)]

[![image14](the-entity-framework-and-aspnet-getting-started-part-7/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image3.png)

`Student` Entitäten verfügen über vier Eigenschaften: `PersonID`, `LastName`, `FirstName`und `EnrollmentDate`. Die Datenbank generiert den ID-Wert automatisch, und die gespeicherte Prozedur akzeptiert Parameter für die anderen drei. Die gespeicherte Prozedur gibt den Wert des Daten Satz Schlüssels der neuen Zeile zurück, damit der Entity Framework den Wert in der Version der Entität nachverfolgen kann, die er im Arbeitsspeicher speichert.

Speichern und schließen Sie das Fenster gespeicherte Prozedur.

Erstellen Sie eine gespeicherte Prozedur `InsertInstructor` auf die gleiche Weise, und verwenden Sie dabei die folgenden SQL-Anweisungen:

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample2.sql)]

Erstellen Sie `Update` gespeicherten Prozeduren auch für `Student`-und `Instructor` Entitäten. (In der Datenbank ist bereits eine gespeicherte Prozedur `DeletePerson` vorhanden, die sowohl für `Instructor` als auch für `Student` Entitäten verwendet werden kann.)

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample3.sql)]

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample4.sql)]

In diesem Tutorial ordnen Sie alle drei Funktionen (INSERT, Update und DELETE) für jeden Entitätstyp zu. Mit dem Entity Framework Version 4 können Sie nur eine oder zwei dieser Funktionen gespeicherten Prozeduren zuordnen, ohne die anderen zuzuordnen, mit einer Ausnahme: Wenn Sie die Update-Funktion, jedoch nicht die Delete-Funktion zuordnen, löst die Entity Framework eine Ausnahme aus, wenn Sie Es wurde versucht, eine Entität zu löschen. In der Entity Framework Version 3,5 gab es bei der Zuordnung gespeicherter Prozeduren keine sehr hohe Flexibilität: Wenn Sie eine Funktion zugeordnet haben, mussten Sie alle drei zuordnen.

Um eine gespeicherte Prozedur zu erstellen, die Daten liest, anstatt Daten zu aktualisieren, erstellen Sie eine, die alle `Course` Entitäten mit den folgenden SQL-Anweisungen auswählt:

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample5.sql)]

## <a name="adding-the-stored-procedures-to-the-data-model"></a>Hinzufügen der gespeicherten Prozeduren zum Datenmodell

Die gespeicherten Prozeduren werden nun in der Datenbank definiert, müssen jedoch dem Datenmodell hinzugefügt werden, um Sie für die Entity Framework verfügbar zu machen. Öffnen Sie *School Model. edmx*, klicken Sie mit der rechten Maustaste auf die Entwurfs Oberfläche, und wählen Sie **Modell aus Datenbank aktualisieren aus**. Erweitern Sie im Dialogfeld **Datenbankobjekte auswählen** auf der Registerkarte **Hinzufügen** den Eintrag **gespeicherte Prozeduren**, wählen Sie die neu erstellten gespeicherten Prozeduren und die gespeicherte Prozedur `DeletePerson` aus, und klicken Sie dann auf **Fertig**stellen

[![image20](the-entity-framework-and-aspnet-getting-started-part-7/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image5.png)

## <a name="mapping-the-stored-procedures"></a>Zuordnung der gespeicherten Prozeduren

Klicken Sie im Datenmodell-Designer mit der rechten Maustaste auf die `Student` Entität, und wählen Sie **Zuordnung der gespeicherten Prozedur**

[![image21](the-entity-framework-and-aspnet-getting-started-part-7/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image7.png)

Das Fenster **Mappingdetails** wird angezeigt. in diesem Fenster können Sie gespeicherte Prozeduren angeben, die vom Entity Framework zum Einfügen, aktualisieren und Löschen von Entitäten dieses Typs verwendet werden sollen.

[![image22](the-entity-framework-and-aspnet-getting-started-part-7/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image9.png)

Legen Sie die **Insert** -Funktion auf **insertstudent**fest. Das Fenster zeigt eine Liste von Parametern gespeicherter Prozeduren an, die jeweils einer Entitäts Eigenschaft zugeordnet werden müssen. Zwei davon werden automatisch zugeordnet, da die Namen identisch sind. Es gibt keine Entitäts Eigenschaft mit dem Namen `FirstName`. Daher müssen Sie `FirstMidName` manuell aus einer Dropdown Liste auswählen, in der die verfügbaren Entitäts Eigenschaften angezeigt werden. (Dies liegt daran, dass Sie den Namen der `FirstName`-Eigenschaft in `FirstMidName` im ersten Tutorial geändert haben.)

[![image23](the-entity-framework-and-aspnet-getting-started-part-7/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image11.png)

Ordnen Sie im gleichen mappingdetailsfenster die `Update`-Funktion der gespeicherten Prozedur `UpdateStudent` zu (stellen Sie sicher, dass Sie `FirstMidName` wie für die gespeicherte Prozedur `Insert` als Parameterwert für `FirstName`und die `Delete`-Funktion für die gespeicherte Prozedur `DeletePerson` angeben.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-7/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image13.png)

Befolgen Sie dasselbe Verfahren, um die gespeicherten Prozeduren INSERT, Update und DELETE für Dozenten der `Instructor`-Entität zuzuordnen.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-7/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image15.png)

Für gespeicherte Prozeduren, die Daten lesen und nicht aktualisieren, verwenden Sie das Fenster **Modell Browser** , um die gespeicherte Prozedur dem zurückgegebenen Entitätstyp zuzuordnen. Klicken Sie im Datenmodell-Designer mit der rechten Maustaste auf die Entwurfs Oberfläche, und wählen Sie **Modell Browser**aus. Öffnen Sie den Knoten **SchoolModel. Store** , und öffnen Sie dann den Knoten **gespeicherte Prozeduren** . Klicken Sie mit der rechten Maustaste auf die gespeicherte Prozedur `GetCourses` und wählen Sie **Funktions Import hinzufügen**aus.

[![image24](the-entity-framework-and-aspnet-getting-started-part-7/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image17.png)

Im Dialogfeld **Funktions Import hinzufügen** unter **gibt eine Auflistung von** Select- **Entitäten**zurück, und wählen Sie dann `Course` als Entitätstyp zurück. Wenn Sie fertig sind, klicken Sie auf **OK**. Speichern und schließen Sie die *edmx* -Datei.

[![image25](the-entity-framework-and-aspnet-getting-started-part-7/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image19.png)

## <a name="using-insert-update-and-delete-stored-procedures"></a>Verwenden gespeicherter Prozeduren INSERT, Update und DELETE

Gespeicherte Prozeduren zum Einfügen, aktualisieren und Löschen von Daten werden automatisch von der Entity Framework verwendet, nachdem Sie Sie dem Datenmodell hinzugefügt und den entsprechenden Entitäten zugeordnet haben. Sie können jetzt die Seite " *studentsadd. aspx* " ausführen, und jedes Mal, wenn Sie einen neuen Studenten erstellen, verwendet die Entity Framework die gespeicherte Prozedur `InsertStudent`, um die neue Zeile der `Student` Tabelle hinzuzufügen.

[![image03](the-entity-framework-and-aspnet-getting-started-part-7/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image21.png)

Führen Sie die Seite *students. aspx* aus, und der neue Student wird in der Liste angezeigt.

[![image04](the-entity-framework-and-aspnet-getting-started-part-7/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image23.png)

Ändern Sie den Namen, um zu überprüfen, ob die Funktion Update funktioniert, und löschen Sie dann den Studenten, um sicherzustellen, dass die Funktion Delete funktioniert.

[![image05](the-entity-framework-and-aspnet-getting-started-part-7/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image25.png)

## <a name="using-select-stored-procedures"></a>Verwenden von gespeicherten Prozeduren auswählen

In der Entity Framework werden gespeicherte Prozeduren wie `GetCourses`nicht automatisch ausgeführt, und Sie können nicht mit dem `EntityDataSource`-Steuerelement verwendet werden. Um Sie zu verwenden, werden Sie aus dem Code aufgerufen.

Öffnen Sie die Datei *InstructorsCourses.aspx.cs* . Die `PopulateDropDownLists`-Methode verwendet eine LINQ-to-Entities-Abfrage, um alle Kurs Entitäten abzurufen, sodass Sie die Liste durchlaufen und feststellen kann, welchen eine der Dozenten zugewiesen ist und welche nicht zugewiesen sind:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample6.cs)]

Ersetzen Sie dies durch den folgenden Code:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample7.cs)]

Die Seite verwendet nun die gespeicherte Prozedur `GetCourses`, um die Liste aller Kurse abzurufen. Führen Sie die Seite aus, um zu überprüfen, ob Sie wie zuvor funktioniert hat.

(Die Navigations Eigenschaften von Entitäten, die von einer gespeicherten Prozedur abgerufen werden, werden möglicherweise nicht automatisch mit den Daten aufgefüllt, die sich auf diese Entitäten beziehen, abhängig von `ObjectContext` Standardeinstellungen Weitere Informationen finden Sie unter [Laden verwandter Objekte](https://msdn.microsoft.com/library/bb896272.aspx) in der MSDN Library.)

Im nächsten Tutorial erfahren Sie, wie Sie dynamische Daten-Funktionalität verwenden, um das Programmieren und Testen von Daten Formatierungs-und Validierungsregeln zu vereinfachen. Anstatt für jede Webseiten Regel, wie z. b. Datenformat Zeichenfolgen, anzugeben, und unabhängig davon, ob ein Feld erforderlich ist, können Sie diese Regeln in den Metadaten des Datenmodells angeben, die automatisch auf jeder Seite angewendet werden.

> [!div class="step-by-step"]
> [Zurück](the-entity-framework-and-aspnet-getting-started-part-6.md)
> [Weiter](the-entity-framework-and-aspnet-getting-started-part-8.md)
