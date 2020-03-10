---
uid: web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
title: Integrieren von jQuery UI DatePicker in die Modell Bindung und Web Forms | Microsoft-Dokumentation
author: Rick-Anderson
description: In dieser tutorialreihe werden grundlegende Aspekte der Verwendung der Modell Bindung mit einem ASP.net Web Forms-Projekt veranschaulicht. Die Modell Bindung führt zu einer geraden Daten Interaktion-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 3cbab37b-fb0f-4751-9ec4-74e068c3f380
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: c8d711dd44950116f3a3e09d5d12c507918c543f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78521979"
---
# <a name="integrating-jquery-ui-datepicker-with-model-binding-and-web-forms"></a>Integrieren von jQuery UI DatePicker in die Modell Bindung und Web Forms

von [Tom fitzmacken](https://github.com/tfitzmac)

> In dieser tutorialreihe werden grundlegende Aspekte der Verwendung der Modell Bindung mit einem ASP.net Web Forms-Projekt veranschaulicht. Die Modell Bindung sorgt für eine genauere Daten Interaktion als bei der Verarbeitung von Datenquellen Objekten (z. b. ObjectDataSource oder SqlDataSource). Diese Serie beginnt mit Einführungs Material und wechselt in spätere Tutorials zu erweiterten Konzepten.
> 
> In diesem Tutorial wird gezeigt, wie Sie das jQuery UI [DatePicker-Widget](http://jqueryui.com/datepicker/) einem Webformular hinzufügen und die Modell Bindung verwenden, um die Datenbank mit dem ausgewählten Wert zu aktualisieren.
> 
> Dieses Tutorial baut auf dem Projekt auf, das in den [ersten](retrieving-data.md) und [zweiten](updating-deleting-and-creating-data.md) Teilen der Reihe erstellt wurde.
> 
> Sie können das gesamte Projekt in C# oder VB [herunterladen](https://go.microsoft.com/fwlink/?LinkId=286116) . Der herunterladbare Code funktioniert entweder mit Visual Studio 2012 oder Visual Studio 2013. Dabei wird die Vorlage Visual Studio 2012 verwendet, die sich geringfügig von der in diesem Tutorial gezeigten Visual Studio 2013 Vorlage unterscheidet.

## <a name="what-youll-build"></a>Was Sie erstellen

In diesem Tutorial gehen Sie wie folgt vor:

1. Fügen Sie dem Modell eine Eigenschaft hinzu, um das Registrierungsdatum des Studenten aufzuzeichnen.
2. Ermöglicht dem Benutzer die Auswahl des anmeldeanmeldedatums mithilfe des jQuery UI DatePicker-Widgets.
3. Erzwingen von Validierungsregeln für das anmelderungs Datum

Das jQuery UI DatePicker-Widget ermöglicht es Benutzern, auf einfache Weise ein Datum aus einem Kalender auszuwählen, der angezeigt wird, wenn der Benutzer mit dem Feld interagiert. Das Verwenden dieses Widgets kann für Benutzer bequemer sein als das manuelle Eingeben eines Datums. Die Integration des DatePicker-Widgets in eine Seite, die die Modell Bindung für Daten Vorgänge verwendet, erfordert nur eine kleine Menge zusätzlicher Aufgaben.

## <a name="add-a-new-property-to-the-model"></a>Neue Eigenschaft zum Modell hinzufügen

Zuerst fügen Sie Ihrem Studenten Modell eine **DateTime** -Eigenschaft hinzu und migrieren diese Änderung in die Datenbank. Öffnen Sie **UniversityModels.cs**, und fügen Sie dem Student-Modell den hervorgehobenen Code hinzu.

[!code-csharp[Main](integrating-jquery-ui/samples/sample1.cs?highlight=16-18)]

Das **RangeAttribute-Attribut** ist enthalten, um Validierungsregeln für die Eigenschaft zu erzwingen. Für dieses Tutorial gehen wir davon aus, dass die "Configuration Manager-Universität" am 1. Januar 2013 gegründet wurde und daher frühere Registrierungsdaten nicht gültig sind.

Fügen Sie im Paketverwaltung Fenster eine Migration hinzu, indem Sie den Befehl **Add-Migration addenrollmentdate**ausführen. Beachten Sie, dass der Migrations Code der Tabelle "Student" die neue Spalte "DateTime" hinzufügt. Um den Wert abzugleichen, den Sie in RangeAttribute angegeben haben, fügen Sie einen Standardwert für die neue Spalte hinzu, wie im folgenden hervorgehobenen Code gezeigt.

[!code-csharp[Main](integrating-jquery-ui/samples/sample2.cs?highlight=11)]

Speichern Sie die Änderungen an der Migrations Datei.

Sie müssen die Daten nicht erneut als Ausgangswert für die Daten verwenden. Öffnen Sie daher **Configuration.cs** im Migrations Ordner, und entfernen Sie den Code in der **Seed** -Methode, oder kommentieren Sie ihn aus. Speichern und schließen Sie die Datei.

Führen Sie nun den Befehl **Update-Database**aus. Beachten Sie, dass die Spalte jetzt in der Datenbank vorhanden ist und dass alle vorhandenen Datensätze den Standardwert für "anmelmentdate" aufweisen.

## <a name="add-dynamic-controls-for-enrollment-date"></a>Dynamische Steuerelemente zum Registrierungsdatum hinzufügen

Nun fügen Sie Steuerelemente zum Anzeigen und Bearbeiten des Anmeldedatums hinzu. An diesem Punkt wird der Wert in einem Textfeld bearbeitet. Später in diesem Tutorial ändern Sie das Textfeld in das jQuery-DatePicker-Widget.

Zunächst ist es wichtig zu beachten, dass Sie keine Änderungen an der **addstudent. aspx** -Datei vornehmen müssen. Die neue Eigenschaft wird automatisch durch das DynamicEntity-Steuerelement dargestellt.

Öffnen Sie **students. aspx**, und fügen Sie den folgenden hervorgehobenen Code hinzu.

[!code-aspx[Main](integrating-jquery-ui/samples/sample3.aspx?highlight=13)]

Führen Sie die Anwendung aus, und beachten Sie, dass Sie den Wert des Anmeldungs Datums festlegen können, indem Sie ein Datum eingeben. Beim Hinzufügen eines neuen Studenten:

![Datum festlegen](integrating-jquery-ui/_static/image1.png)

Oder Bearbeiten eines vorhandenen Werts:

![Datum bearbeiten](integrating-jquery-ui/_static/image2.png)

Das Eingeben des Datums funktioniert, ist jedoch möglicherweise nicht die Kundenfreundlichkeit, die Sie bereitstellen möchten. Im nächsten Abschnitt aktivieren Sie die Auswahl eines Datums in einem Kalender.

## <a name="install-nuget-package-to-work-with-jquery-ui"></a>Installieren des nuget-Pakets für die Verwendung der jQuery-Benutzeroberfläche

Das nuget-Paket für die Benutzer **Oberfläche von Juice** ermöglicht eine einfache Integration der jQuery-UI-Widgets in Ihre Webanwendung. Um dieses Paket zu verwenden, installieren Sie es über nuget.

![Hinzufügen einer Benutzeroberfläche](integrating-jquery-ui/_static/image3.png)

Die von Ihnen installierte Version der Juice-Benutzeroberfläche kann einen Konflikt mit der Version von jQuery in der Anwendung verursachen. Bevor Sie mit diesem Tutorial fortfahren, versuchen Sie, die Anwendung auszuführen. Wenn ein JavaScript-Fehler auftritt, müssen Sie die jQuery-Version abstimmen. Sie können entweder die erwartete Version von jQuery zum Skript Ordner hinzufügen (Version 1.8.2 zum Zeitpunkt des Schreibens dieses Tutorials) oder in Site. Master den Pfad zu der jQuery-Datei angeben.

[!code-aspx[Main](integrating-jquery-ui/samples/sample4.aspx)]

## <a name="customize-datetime-template-to-include-datepicker-widget"></a>Anpassen der DateTime-Vorlage an das DatePicker-widget

Sie fügen das DatePicker-Widget zur Vorlage für dynamische Daten hinzu, um einen DateTime-Wert zu bearbeiten. Indem das Widget der Vorlage hinzugefügt wird, wird es automatisch in der Form zum Hinzufügen eines neuen Studenten und in der Rasteransicht zum Bearbeiten von Schülern gerendert. Öffnen Sie **DateTime\_"Edit. ascx**", und fügen Sie den folgenden hervorgehobenen Code hinzu.

[!code-aspx[Main](integrating-jquery-ui/samples/sample5.aspx?highlight=3)]

In der Code Behind-Datei legen Sie die minimalen und maximalen Datumsangaben für "DatePicker" fest. Wenn Sie diese Werte festlegen, werden Benutzer daran gehindert, zu ungültigen Datumsangaben zu navigieren. Sie rufen die minimalen und maximalen Werte aus **RangeAttribute** der DateTime-Eigenschaft ab, sofern vorhanden. Öffnen Sie **DateTime-\_Edit.ascx.cs**, und fügen Sie der Seite\_Load-Methode den folgenden hervorgehobenen Code hinzu.

[!code-csharp[Main](integrating-jquery-ui/samples/sample6.cs?highlight=9-14)]

Führen Sie die-Webanwendung aus, und navigieren Sie zur addstudent-Seite. Geben Sie Werte für die Felder an. Beachten Sie, dass beim Klicken auf das Textfeld für das Registrierungsdatum der Kalender angezeigt wird.

![Datumsauswahl](integrating-jquery-ui/_static/image4.png)

Wählen Sie ein Datum aus, und klicken Sie auf **Einfügen**. Das RangeAttribute erzwingt die Validierung auf dem Server. Durch Festlegen der MinDate-Eigenschaft für DatePicker wenden Sie außerdem eine Validierung auf dem Client an. Der Kalender ermöglicht dem Benutzer nicht, zu einem Datum vor dem Wert von MinDate zu navigieren.

Wenn Sie einen Datensatz in der Rasteransicht bearbeiten, wird auch der Kalender angezeigt.

![DatePicker in GridView](integrating-jquery-ui/_static/image5.png)

## <a name="conclusion"></a>Zusammenfassung

In diesem Tutorial haben Sie gelernt, wie Sie ein jQuery-Widget in ein Webformular einbinden, das eine Modell Bindung verwendet.

Im nächsten [Tutorial](using-query-string-values-to-retrieve-data.md)verwenden Sie einen Abfrage Zeichen folgen Wert, wenn Sie Daten auswählen.

> [!div class="step-by-step"]
> [Zurück](sorting-paging-and-filtering-data.md)
> [Weiter](using-query-string-values-to-retrieve-data.md)
