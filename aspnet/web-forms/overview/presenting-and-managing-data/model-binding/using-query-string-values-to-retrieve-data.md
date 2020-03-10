---
uid: web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
title: Verwenden von Abfrage Zeichenfolgen-Werten zum Filtern von Daten mit Modell Bindung und Web Forms | Microsoft-Dokumentation
author: Rick-Anderson
description: In dieser tutorialreihe werden grundlegende Aspekte der Verwendung der Modell Bindung mit einem ASP.net Web Forms-Projekt veranschaulicht. Die Modell Bindung führt zu einer geraden Daten Interaktion-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: b90978bd-795d-4871-9ade-1671caff5730
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
msc.type: authoredcontent
ms.openlocfilehash: 143ddcb40b576a3129e659b90bfc8321c061a547
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78519081"
---
# <a name="using-query-string-values-to-filter-data-with-model-binding-and-web-forms"></a>Verwenden von Abfrage Zeichenfolgen-Werten zum Filtern von Daten mit Modell Bindung und Web Forms

von [Tom fitzmacken](https://github.com/tfitzmac)

> In dieser tutorialreihe werden grundlegende Aspekte der Verwendung der Modell Bindung mit einem ASP.net Web Forms-Projekt veranschaulicht. Die Modell Bindung sorgt für eine genauere Daten Interaktion als bei der Verarbeitung von Datenquellen Objekten (z. b. ObjectDataSource oder SqlDataSource). Diese Serie beginnt mit Einführungs Material und wechselt in spätere Tutorials zu erweiterten Konzepten.
> 
> In diesem Tutorial wird gezeigt, wie ein Wert in der Abfrage Zeichenfolge übergeben wird und wie dieser Wert zum Abrufen von Daten über die Modell Bindung verwendet wird.
> 
> Dieses Tutorial baut auf dem Projekt auf, das in den [vorherigen](retrieving-data.md) Teilen der Reihe erstellt wurde.
> 
> Sie können das gesamte Projekt in C# oder VB [herunterladen](https://go.microsoft.com/fwlink/?LinkId=286116) . Der herunterladbare Code funktioniert entweder mit Visual Studio 2012 oder Visual Studio 2013. Dabei wird die Vorlage Visual Studio 2012 verwendet, die sich geringfügig von der in diesem Tutorial gezeigten Visual Studio 2013 Vorlage unterscheidet.

## <a name="what-youll-build"></a>Was Sie erstellen

In diesem Tutorial gehen Sie wie folgt vor:

1. Fügen Sie eine neue Seite hinzu, um die registrierten Kurse für einen Schüler/Student anzuzeigen.
2. Registrierte Kurse für den ausgewählten Studenten basierend auf einem Wert in der Abfrage Zeichenfolge abrufen
3. Fügen Sie der neuen Seite einen Hyperlink mit einem Abfrage Zeichen folgen Wert aus der Rasteransicht hinzu.

Die Schritte in diesem Tutorial ähneln denen im vorherigen [Tutorial](sorting-paging-and-filtering-data.md) zum Filtern der angezeigten Studenten basierend auf der Benutzer Auswahl in einer Dropdown Liste. In diesem Tutorial haben Sie das **Control** -Attribut in der Select-Methode verwendet, um anzugeben, dass der Parameterwert von einem-Steuerelement stammt. In diesem Tutorial verwenden Sie das Attribut **QueryString** in der Select-Methode, um anzugeben, dass der Parameterwert aus der Abfrage Zeichenfolge stammt.

## <a name="add-new-page-for-displaying-a-students-courses"></a>Neue Seite zum Anzeigen der Kurse eines Studenten hinzufügen

Fügen Sie ein neues Webformular hinzu, das die Master Seite Site. Master verwendet, und benennen Sie die Seiten **Kurse**.

Fügen Sie in der Datei " **Courses. aspx** " eine Rasteransicht hinzu, um die Kurse für den ausgewählten Studenten anzuzeigen.

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample1.aspx)]

## <a name="define-the-select-method"></a>Definieren der Select-Methode

In **Courses.aspx.cs**fügen Sie die Select-Methode mit dem Namen hinzu, den Sie in der **SelectMethod** -Eigenschaft der Rasteransicht angegeben haben. In dieser Methode definieren Sie die Abfrage zum Abrufen der Kurse eines Studenten und geben an, dass der Parameter von einem Abfrage Zeichen folgen Wert mit dem gleichen Namen wie der Parameter stammt.

Zuerst müssen Sie die folgenden **using** -Anweisungen hinzufügen.

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample2.cs)]

Fügen Sie dann Courses.aspx.cs den folgenden Code hinzu:

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample3.cs)]

Das QueryString-Attribut bedeutet, dass der-Parameter in dieser Methode automatisch ein Abfrage Zeichen folgen Wert mit dem Namen "StudentID" zugewiesen wird.

## <a name="add-hyperlink-with-query-string-value"></a>Hyperlink mit Abfrage Zeichen folgen Wert hinzufügen

In der Rasteransicht auf "students. aspx" fügen Sie ein Hyperlink-Feld hinzu, das mit der neuen Seite "Kurse" verknüpft ist. Der Hyperlink enthält einen Abfrage Zeichen folgen Wert mit der ID des Studenten.

Fügen Sie in students. aspx das folgende Feld zu den Spalten der Rasteransicht direkt unterhalb des Felds für das Gesamtguthaben hinzu.

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample4.aspx?highlight=7-8)]

Führen Sie die Anwendung aus, und beachten Sie, dass die Rasteransicht jetzt den Link "Kurse" enthält.

![Hyperlink hinzufügen](using-query-string-values-to-retrieve-data/_static/image1.png)

Wenn Sie auf einen der Links klicken, sehen Sie die angemeldeten Kurse des Studenten.

![Kurse anzeigen](using-query-string-values-to-retrieve-data/_static/image2.png)

## <a name="conclusion"></a>Zusammenfassung

In diesem Tutorial haben Sie einen Link mit einem Abfrage Zeichen folgen Wert hinzugefügt. Sie haben diesen Abfrage Zeichen folgen Wert für den Parameterwert in der Select-Methode verwendet.

Im nächsten [Tutorial](adding-business-logic-layer.md)verschieben Sie den Code aus den Code-Behind-Dateien in eine Geschäftslogik Schicht und eine Datenzugriffs Schicht.

> [!div class="step-by-step"]
> [Zurück](integrating-jquery-ui.md)
> [Weiter](adding-business-logic-layer.md)
