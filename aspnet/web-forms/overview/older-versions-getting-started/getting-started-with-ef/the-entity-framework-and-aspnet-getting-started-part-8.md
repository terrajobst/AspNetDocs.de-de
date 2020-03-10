---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
title: Die ersten Schritte mit Entity Framework 4,0 Database First und ASP.NET 4 Web Forms-Teil 8 | Microsoft-Dokumentation
author: tdykstra
description: Die Beispiel-Webanwendung der Beispiel-Web-App veranschaulicht, wie Sie mithilfe der Entity Framework Web Forms Anwendungen erstellen. Die Beispielanwendung ist...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: aaadd9bb-5508-448c-ad57-5497dff90e13
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
msc.type: authoredcontent
ms.openlocfilehash: ff6a808dd501df8056c0fb0784a8e42e094d5db7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78473505"
---
# <a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-8"></a>Die ersten Schritte mit Entity Framework 4,0 Database First und ASP.NET 4 Web Forms-Teil 8

von [Tom Dykstra](https://github.com/tdykstra)

> Die Beispiel-Webanwendung der Beispiel-Web-App veranschaulicht, wie Sie ASP.net-Web Forms Anwendungen mithilfe von Entity Framework 4,0 und Visual Studio 2010 erstellen. Weitere Informationen zur tutorialreihe finden Sie [im ersten Tutorial der Reihe](the-entity-framework-and-aspnet-getting-started-part-1.md) .

## <a name="using-dynamic-data-functionality-to-format-and-validate-data"></a>Verwenden von dynamische Daten Funktionen zum Formatieren und Validieren von Daten

Im vorherigen Tutorial haben Sie gespeicherte Prozeduren implementiert. In diesem Tutorial wird gezeigt, wie dynamische Daten Funktionen die folgenden Vorteile bieten können:

- Felder werden automatisch für die Anzeige auf der Grundlage ihres Datentyps formatiert.
- Felder werden automatisch auf Grundlage ihres Datentyps überprüft.
- Sie können dem Datenmodell Metadaten hinzufügen, um Formatierung und Validierungs Verhalten anzupassen. Wenn Sie dies tun, können Sie die Formatierungs-und Validierungsregeln an nur einem Ort hinzufügen. Sie werden automatisch überall dort angewendet, wo Sie mit dynamische Daten-Steuerelementen auf die Felder zugreifen.

Um zu sehen, wie dies funktioniert, ändern Sie die Steuerelemente, die Sie zum Anzeigen und Bearbeiten von Feldern in der vorhandenen Seite " *students. aspx* " verwenden, und fügen Sie Formatierungs-und Validierungs Metadaten zu den Feldern "Name" und "Date" des `Student` Entitäts Typs hinzu.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-8/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image1.png)

## <a name="using-dynamicfield-and-dynamiccontrol-controls"></a>Verwenden von DynamicField-und DynamicControl-Steuerelementen

Öffnen Sie die Seite " *students. aspx* ", und ersetzen Sie im `StudentsGridView` Steuerelement den **Namen** und das Registrierungs **Datum** `TemplateField` Elemente durch das folgende Markup:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample1.aspx)]

Dieses Markup verwendet `DynamicControl` Steuerelemente anstelle von `TextBox` und `Label` Steuerelemente im Feld "Student Name Template", und es wird ein `DynamicField` Steuerelement für das anmeldungstag verwendet. Es wurden keine Format Zeichenfolgen angegeben.

Fügen Sie ein `ValidationSummary`-Steuerelement nach dem `StudentsGridView` Steuerelement hinzu.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample2.aspx)]

Ersetzen Sie im `SearchGridView` Steuerelement das Markup für die Spalten " **Name** " und "Registrierungs **Datum** " wie im `StudentsGridView` Steuerelement, mit der Ausnahme, dass Sie das `EditItemTemplate`-Element weglassen. Das `Columns`-Element des `SearchGridView`-Steuer Elements enthält jetzt das folgende Markup:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample3.aspx)]

Öffnen Sie *students.aspx.cs* , und fügen Sie die folgende `using`-Anweisung hinzu:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample4.cs)]

Fügen Sie einen Handler für das `Init` Ereignis der Seite hinzu:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample5.cs)]

Dieser Code gibt an, dass dynamische Daten in diesen Daten gebundenen Steuerelementen Formatierungen und Validierungen für Felder der `Student` Entität bereitstellt. Wenn Sie eine Fehlermeldung wie im folgenden Beispiel erhalten, wenn Sie die Seite ausführen, bedeutet dies in der Regel, dass Sie vergessen haben, die `EnableDynamicData`-Methode in `Page_Init`aufzurufen:

`Could not determine a MetaTable. A MetaTable could not be determined for the data source 'StudentsEntityDataSource' and one could not be inferred from the request URL.`

Führen Sie die Seite aus.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-8/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image3.png)

In der Spalte **Anmeldungsdatum** wird die Uhrzeit zusammen mit dem Datum angezeigt, weil der Eigenschaftentyp `DateTime`ist. Dies wird später behoben.

Beachten Sie vorerst, dass dynamische Daten automatisch grundlegende Datenvalidierung bereitstellt. Beispiel: Klicken Sie auf **Bearbeiten**, löschen Sie das Feld Datum, klicken Sie auf **Aktualisieren**, und Sie sehen, dass dynamische Daten dieses Feld automatisch als Pflichtfeld macht, da der Wert im Datenmodell nicht zulässig ist. Die Seite zeigt ein Sternchen hinter dem Feld und eine Fehlermeldung im `ValidationSummary` Steuerelement an:

[![Image05](the-entity-framework-and-aspnet-getting-started-part-8/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image5.png)

Sie können das `ValidationSummary` Steuerelement weglassen, da Sie auch den Mauszeiger über dem Sternchen halten können, um die Fehlermeldung anzuzeigen:

[![Image06](the-entity-framework-and-aspnet-getting-started-part-8/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image7.png)

Außerdem wird dynamische Daten überprüft, ob im Feld **anmelddatum** eingegebene Daten ein gültiges Datum ist:

[![Image04](the-entity-framework-and-aspnet-getting-started-part-8/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image9.png)

Wie Sie sehen können, handelt es sich hierbei um eine generische Fehlermeldung. Im nächsten Abschnitt erfahren Sie, wie Sie Nachrichten sowie Validierungs-und Formatierungs Regeln anpassen.

## <a name="adding-metadata-to-the-data-model"></a>Hinzufügen von Metadaten zum Datenmodell

In der Regel möchten Sie die von dynamische Daten bereitgestellte Funktionalität anpassen. Beispielsweise können Sie ändern, wie Daten angezeigt werden, und den Inhalt von Fehlermeldungen. Sie passen in der Regel auch Daten Validierungsregeln an, um mehr Funktionalität bereitzustellen, als dynamische Daten automatisch basierend auf Datentypen bereitstellt. Zu diesem Zweck erstellen Sie partielle Klassen, die Entitäts Typen entsprechen.

Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt **contosouniversity** , wählen Sie **Verweis hinzufügen**aus, und fügen Sie einen Verweis auf `System.ComponentModel.DataAnnotations`hinzu.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-8/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image11.png)

Erstellen Sie im Ordner *dal* eine neue Klassendatei, nennen Sie Sie *Student.cs*, und ersetzen Sie den Vorlagen Code darin durch den folgenden Code.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample6.cs)]

Mit diesem Code wird eine partielle Klasse für die `Student` Entität erstellt. Das `MetadataType` Attribut, das auf diese partielle Klasse angewendet wird, identifiziert die Klasse, die Sie zum Angeben von Metadaten verwenden. Die Metadatenklasse kann einen beliebigen Namen haben, aber die Verwendung des Entitäts namens Plus "Metadata" ist eine gängige Vorgehensweise.

Die Attribute, die auf Eigenschaften in der Metadatenklasse angewendet werden, geben Formatierung, Validierung, Regeln und Fehlermeldungen an. Die hier gezeigten Attribute haben folgende Ergebnisse:

- `EnrollmentDate` wird als Datum (ohne Uhrzeit) angezeigt.
- Beide namens Felder dürfen maximal 25 Zeichen lang sein, und es wird eine benutzerdefinierte Fehlermeldung bereitgestellt.
- Beide namens Felder sind erforderlich, und es wird eine benutzerdefinierte Fehlermeldung bereitgestellt.

Führen Sie die Seite *students. aspx* erneut aus, und Sie sehen, dass die Datumsangaben jetzt ohne Zeit angezeigt werden:

[![Image08](the-entity-framework-and-aspnet-getting-started-part-8/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image13.png)

Bearbeiten Sie eine Zeile, und versuchen Sie, die Werte in den namens Feldern zu löschen. Die Sternchen, die Feld Fehler anzeigen, werden angezeigt, sobald Sie ein Feld verlassen, bevor Sie auf **Aktualisieren**klicken. Wenn Sie auf **Aktualisieren**klicken, wird der von Ihnen angegebene Fehlermeldungs Text auf der Seite angezeigt.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-8/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image15.png)

Versuchen Sie, Namen mit mehr als 25 Zeichen einzugeben, klicken Sie auf **Aktualisieren**, und auf der Seite wird der von Ihnen angegebene Fehlermeldungs Text angezeigt.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-8/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image17.png)

Nachdem Sie diese Formatierungs-und Validierungsregeln in den Datenmodell Metadaten eingerichtet haben, werden die Regeln automatisch auf jede Seite angewendet, auf der Änderungen an diesen Feldern angezeigt oder zugelassen werden, solange Sie `DynamicControl` oder `DynamicField` Steuerelemente verwenden. Dadurch wird die Menge des redundanten Codes, den Sie schreiben müssen, reduziert, was das Programmieren und testen vereinfacht, und es wird sichergestellt, dass die Datenformatierung und-Validierung in einer Anwendung konsistent sind.

## <a name="more-information"></a>Weitere Informationen

Dies schließt diese Reihe von Tutorials zu den ersten Schritten mit dem Entity Framework. Weitere Ressourcen, die Ihnen die Verwendung des Entity Framework erleichtern, finden Sie [im ersten Tutorial in der nächsten Entity Framework tutorialreihe](../continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md) , oder besuchen Sie die folgenden Websites:

- [FAQ zu Entity Framework](http://www.ef-faq.org/introduction.html)
- [Der Entity Framework Teamblog](https://blogs.msdn.com/b/adonet/)
- [Entity Framework in der MSDN Library](https://msdn.microsoft.com/library/bb399572.aspx)
- [Entity Framework im MSDN-Daten Entwickler Center](https://msdn.microsoft.com/data/ef.aspx)
- [Übersicht über das EntityDataSource-Webserver Steuerelement in der MSDN Library](https://msdn.microsoft.com/library/cc488502.aspx)
- [Referenz zur EntityDataSource-Steuerungs-API in der MSDN Library](https://msdn.microsoft.com/library/system.web.ui.webcontrols.entitydatasource.aspx)
- [Entity Framework Foren auf MSDN](https://social.msdn.microsoft.com/forums/adodotnetentityframework/)
- [Julie Lerman Blog](http://thedatafarm.com/blog/)

> [!div class="step-by-step"]
> [Previous](the-entity-framework-and-aspnet-getting-started-part-7.md)
