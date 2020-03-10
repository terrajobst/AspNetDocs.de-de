---
uid: web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
title: Aktualisieren, löschen und Erstellen von Daten mit Modell Bindung und Web Forms | Microsoft-Dokumentation
author: Rick-Anderson
description: In dieser tutorialreihe werden grundlegende Aspekte der Verwendung der Modell Bindung mit einem ASP.net Web Forms-Projekt veranschaulicht. Die Modell Bindung führt zu einer geraden Daten Interaktion-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 602baa94-5a4f-46eb-a717-7a9e539c1db4
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
msc.type: authoredcontent
ms.openlocfilehash: 11dc52ec79ca91119b37ea60e08164eb1b1d0e2b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78474135"
---
# <a name="updating-deleting-and-creating-data-with-model-binding-and-web-forms"></a>Aktualisieren, löschen und Erstellen von Daten mit Modell Bindung und Web Forms

von [Tom fitzmacken](https://github.com/tfitzmac)

> In dieser tutorialreihe werden grundlegende Aspekte der Verwendung der Modell Bindung mit einem ASP.net Web Forms-Projekt veranschaulicht. Die Modell Bindung sorgt für eine genauere Daten Interaktion als bei der Verarbeitung von Datenquellen Objekten (z. b. ObjectDataSource oder SqlDataSource). Diese Serie beginnt mit Einführungs Material und wechselt in spätere Tutorials zu erweiterten Konzepten.
> 
> In diesem Tutorial wird gezeigt, wie Sie Daten mit Modell Bindung erstellen, aktualisieren und löschen. Legen Sie die folgenden Eigenschaften fest:
> 
> - DeleteMethod
> - InsertMethod
> - UpdateMethod
> 
> Diese Eigenschaften erhalten den Namen der Methode, die den entsprechenden Vorgang behandelt. Innerhalb dieser Methode stellen Sie die Logik zum interagieren mit den Daten bereit.
> 
> Dieses Tutorial baut auf dem Projekt auf, das im ersten [Teil](retrieving-data.md) der Reihe erstellt wurde.
> 
> Sie können das gesamte Projekt in C# oder VB [herunterladen](https://go.microsoft.com/fwlink/?LinkId=286116) . Der herunterladbare Code funktioniert entweder mit Visual Studio 2012 oder Visual Studio 2013. Dabei wird die Vorlage Visual Studio 2012 verwendet, die sich geringfügig von der in diesem Tutorial gezeigten Visual Studio 2013 Vorlage unterscheidet.

## <a name="what-youll-build"></a>Was Sie erstellen

In diesem Tutorial gehen Sie wie folgt vor:

1. Hinzufügen dynamischer Datenvorlagen
2. Aktivieren des Aktualisierens und Löschens von Daten durch Modell Bindungsmethoden
3. Anwenden von Daten Validierungsregeln: Aktivieren Sie das Erstellen eines neuen Datensatzes in der Datenbank.

## <a name="add-dynamic-data-templates"></a>Hinzufügen dynamischer Datenvorlagen

Zum Bereitstellen der optimalen Benutzer Leistung und Minimieren der Code Wiederholung verwenden Sie dynamische Datenvorlagen. Sie können vorgefertigte dynamische Datenvorlagen problemlos in Ihre vorhandene Site integrieren, indem Sie ein nuget-Paket installieren.

Installieren Sie die **dynamicdatatemplatescs**über die **nuget-Pakete verwalten**.

![dynamische Datenvorlagen](updating-deleting-and-creating-data/_static/image1.png)

Beachten Sie, dass Ihr Projekt jetzt einen Ordner mit dem Namen " **DynamicData**" enthält. In diesem Ordner finden Sie die Vorlagen, die automatisch auf dynamische Steuerelemente in ihren Webformularen angewendet werden.

![Ordner für dynamische Daten](updating-deleting-and-creating-data/_static/image2.png)

## <a name="enable-updating-and-deleting"></a>Aktualisieren und löschen aktivieren

Das Aktualisieren und Löschen von Datensätzen in der Datenbank durch Benutzer ist dem Prozess zum Abrufen von Daten sehr ähnlich. In den Eigenschaften **UpdateMethod** und **DeleteMethod** geben Sie die Namen der Methoden an, die diese Vorgänge ausführen. Mit einem GridView-Steuerelement können Sie auch die automatische Generierung von Schaltflächen zum Bearbeiten und Löschen angeben. Der folgende markierte Code zeigt die Ergänzungen zum GridView-Code.

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample1.aspx?highlight=4-5)]

Fügen Sie in der Code Behind-Datei eine using-Anweisung für **System. Data. Entity. Infrastructure**hinzu.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample2.cs)]

Fügen Sie dann die folgenden Aktualisierungs-und Löschmethoden hinzu.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample3.cs)]

Die **tryupdatemodel** -Methode wendet die übereinstimmenden Daten gebundenen Werte aus dem Webformular auf das Datenelement an. Das Datenelement wird basierend auf dem Wert des ID-Parameters abgerufen.

## <a name="enforce-validation-requirements"></a>Validierungsanforderungen erzwingen

Die Validierungs Attribute, die Sie auf die Eigenschaften "FirstName", "LastName" und "Year" in der Klasse "Student" angewendet haben, werden beim Aktualisieren der Daten automatisch erzwungen. Die DynamicField-Steuerelemente fügen Client-und Server Validierungs Steuerelemente basierend auf den Validierungs Attributen hinzu. Die Eigenschaften FirstName und LastName sind beide erforderlich. FirstName darf nicht länger als 20 Zeichen sein, und LastName darf nicht länger als 40 Zeichen sein. Year muss ein gültiger Wert für die "akadecyear"-Enumeration sein.

Wenn der Benutzer gegen eine der Überprüfungsanforderungen verstößt, wird das Update nicht fortgesetzt. Fügen Sie über der GridView ein ValidationSummary-Steuerelement hinzu, um die Fehlermeldung anzuzeigen. Legen Sie die Eigenschaft **showmodelstateerrors** auf **true**fest, um die Validierungs Fehler aus der Modell Bindung anzuzeigen. 

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample4.aspx)]

Führen Sie die-Webanwendung aus, und aktualisieren und löschen Sie alle Datensätze.

![Aktualisieren von Daten](updating-deleting-and-creating-data/_static/image3.png)

Beachten Sie, dass der Wert für die Year-Eigenschaft im Bearbeitungsmodus automatisch als Dropdown Liste gerendert wird. Die Year-Eigenschaft ist ein Enumerationswert, und die Vorlage für dynamische Daten für einen Enumerationswert gibt eine Dropdown Liste zum Bearbeiten an. Sie finden diese Vorlage, indem Sie die- **Enumeration\_Datei "Edit. ascx** " im Ordner " **DynamicData**/**FieldTemplates** " öffnen.

Wenn Sie gültige Werte angeben, wird das Update erfolgreich abgeschlossen. Wenn Sie gegen eine der Überprüfungsanforderungen verstoßen, wird das Update nicht fortgesetzt, und es wird eine Fehlermeldung oberhalb des Rasters angezeigt.

![Fehlermeldung](updating-deleting-and-creating-data/_static/image4.png)

## <a name="add-new-records"></a>Neue Datensätze hinzufügen

Das GridView-Steuerelement enthält nicht die **InsertMethod** -Eigenschaft und kann daher nicht zum Hinzufügen eines neuen Datensatzes mit Modell Bindung verwendet werden. Sie finden die InsertMethod-Eigenschaft in den Steuerelementen **FormView**, **DetailsView**oder **ListView** . In diesem Tutorial verwenden Sie ein FormView-Steuerelement, um einen neuen Datensatz hinzuzufügen.

Fügen Sie zunächst einen Link zur neuen Seite hinzu, die Sie zum Hinzufügen eines neuen Datensatzes erstellen. Fügen Sie oberhalb von ValidationSummary Folgendes hinzu:

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample5.aspx)]

Der neue Link wird am oberen Rand des Inhalts für die Seite "Studenten" angezeigt.

![neuer Link](updating-deleting-and-creating-data/_static/image5.png)

Fügen Sie dann ein neues Webformular mithilfe einer Master Seite hinzu, und nennen Sie es " **addstudent**". Wählen Sie Site. Master als Master Seite aus.

Sie werden die Felder zum Hinzufügen eines neuen Studenten mithilfe eines **dynamicenti-** Steuer Elements gereinigen. Das dynamikty-Steuerelement rendert diese bearbeitbaren Eigenschaften in der Klasse, die in der ItemType-Eigenschaft angegeben ist. Die StudentID-Eigenschaft wurde mit dem Attribut **[gerüstcolumn (false)]** gekennzeichnet, sodass es nicht gerendert wird. Fügen Sie im Platzhalter mainContent der Seite addstudent den folgenden Code hinzu.

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample6.aspx)]

Fügen Sie in der Code Behind-Datei (AddStudent.aspx.cs) eine **using** -Anweisung für den **contosouniversitymodelbinding. Models** -Namespace hinzu.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample7.cs)]

Fügen Sie dann die folgenden Methoden hinzu, um anzugeben, wie ein neuer Datensatz und ein Ereignishandler für die Schaltfläche Abbrechen eingefügt werden sollen.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample8.cs)]

Speichern Sie alle Änderungen.

Führen Sie die Webanwendung aus, und erstellen Sie einen neuen Studenten.

![neuen Studenten hinzufügen](updating-deleting-and-creating-data/_static/image6.png)

Klicken Sie auf **Einfügen** , und beachten Sie, dass der neue Student erstellt wurde.

![neuen Studenten anzeigen](updating-deleting-and-creating-data/_static/image7.png)

## <a name="conclusion"></a>Zusammenfassung

In diesem Tutorial haben Sie das Aktualisieren, löschen und Erstellen von Daten ermöglicht. Sie haben bei der Interaktion mit den Daten sichergestellt, dass Validierungsregeln angewendet werden.

Im nächsten [Tutorial](sorting-paging-and-filtering-data.md) dieser Reihe aktivieren Sie das Sortieren, Paging und das Filtern von Daten.

> [!div class="step-by-step"]
> [Zurück](retrieving-data.md)
> [Weiter](sorting-paging-and-filtering-data.md)
