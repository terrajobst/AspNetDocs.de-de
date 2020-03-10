---
uid: web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
title: Sortieren, Paging und Filtern von Daten mit Modell Bindung und Web Forms | Microsoft-Dokumentation
author: Rick-Anderson
description: In dieser tutorialreihe werden grundlegende Aspekte der Verwendung der Modell Bindung mit einem ASP.net Web Forms-Projekt veranschaulicht. Die Modell Bindung führt zu einer geraden Daten Interaktion-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 266e7866-e327-4687-b29d-627a0925e87d
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
msc.type: authoredcontent
ms.openlocfilehash: f8e64392af6110f36c6af98c4e4e9481c94a0d82
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78441063"
---
# <a name="sorting-paging-and-filtering-data-with-model-binding-and-web-forms"></a>Sortieren, Paging und Filtern von Daten mit Modell Bindung und Web Forms

von [Tom fitzmacken](https://github.com/tfitzmac)

> In dieser tutorialreihe werden grundlegende Aspekte der Verwendung der Modell Bindung mit einem ASP.net Web Forms-Projekt veranschaulicht. Die Modell Bindung sorgt für eine genauere Daten Interaktion als bei der Verarbeitung von Datenquellen Objekten (z. b. ObjectDataSource oder SqlDataSource). Diese Serie beginnt mit Einführungs Material und wechselt in spätere Tutorials zu erweiterten Konzepten.
> 
> In diesem Tutorial wird gezeigt, wie Sie die Daten mithilfe der Modell Bindung sortieren, Paging und Filtern können.
> 
> Dieses Tutorial baut auf dem Projekt auf, das im ersten [Teil](retrieving-data.md) der Reihe erstellt wurde.
> 
> Sie können das gesamte Projekt in C# oder VB [herunterladen](https://go.microsoft.com/fwlink/?LinkId=286116) . Der herunterladbare Code funktioniert entweder mit Visual Studio 2012 oder Visual Studio 2013. Dabei wird die Vorlage Visual Studio 2012 verwendet, die sich geringfügig von der in diesem Tutorial gezeigten Visual Studio 2013 Vorlage unterscheidet.

## <a name="what-youll-build"></a>Was Sie erstellen

In diesem Tutorial gehen Sie wie folgt vor:

1. Sortieren und Paging der Daten aktivieren
2. Ermöglicht das Filtern der Daten basierend auf einer Auswahl durch den Benutzer.

## <a name="add-sorting"></a>Hinzufügen von Sortierung

Das Aktivieren der Sortierung in der GridView ist sehr einfach. Legen Sie in der Datei "Student. aspx" in der GridView einfach " **allowsortier** " auf " **true** " fest. Es ist nicht erforderlich, für jede Spalte einen **SortExpression** -Wert festzulegen, da die Daten aus automatisch verwendet werden. Die GridView ändert die Abfrage so, dass Sie die Reihenfolge der Daten nach dem ausgewählten Wert einschließt. Der hervorgehobene Code unten zeigt, was Sie zum Aktivieren der Sortierung vornehmen müssen.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample1.aspx?highlight=5)]

Führen Sie die Webanwendung aus, und testen Sie das Sortieren von Student-Datensätzen anhand der Werte in verschiedenen Spalten

![Studenten sortieren](sorting-paging-and-filtering-data/_static/image2.png)

## <a name="add-paging"></a>Hinzufügen von Paging

Das Aktivieren von Paging ist ebenfalls sehr einfach. Legen Sie in der GridView die **AllowPaging** -Eigenschaft auf **true** fest, und legen Sie die **PageSize** -Eigenschaft auf die Anzahl der Datensätze fest, die auf jeder Seite angezeigt werden sollen. In diesem Tutorial können Sie es auf 4 festlegen.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample2.aspx?highlight=5)]

Führen Sie die-Webanwendung aus, und beachten Sie, dass die Datensätze nun auf mehrere Seiten aufgeteilt sind und höchstens vier Datensätze auf einer einzelnen Seite angezeigt werden.

![Paging hinzufügen](sorting-paging-and-filtering-data/_static/image4.png)

Durch die verzögerte Abfrage Ausführung wird die Effizienz der Anwendung verbessert. Anstatt das gesamte Dataset abzurufen, ändert die GridView die Abfrage so, dass nur die Datensätze für die aktuelle Seite abgerufen werden.

## <a name="filter-records-by-user-selection"></a>Datensätze nach Benutzer Auswahl Filtern

Die Modell Bindung fügt mehrere Attribute hinzu, mit denen Sie festlegen können, wie der Wert für einen Parameter in einer Modell Bindungsmethode festgelegt wird. Diese Attribute befinden sich im **System. Web. modelbinding** -Namespace. Dazu zählen:

- Control
- Cookie
- Formular
- Profil
- QueryString
- RouteData
- Sitzung
- UserProfile
- ViewState

In diesem Tutorial verwenden Sie den Wert eines Steuer Elements, um zu filtern, welche Datensätze in der GridView angezeigt werden. Sie fügen das **Control** -Attribut der Abfrage Methode hinzu, die Sie zuvor erstellt haben. In einem [späteren](using-query-string-values-to-retrieve-data.md) Tutorial wenden Sie das Attribut **QueryString** auf einen Parameter an, um anzugeben, dass der Parameterwert aus dem Wert einer Abfrage Zeichenfolge stammt.

Fügen Sie zunächst oberhalb von ValidationSummary eine Dropdown Liste hinzu, um zu filtern, welche Studenten angezeigt werden.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample3.aspx?highlight=3-11)]

Ändern Sie in der Code Behind-Datei die Select-Methode so, dass Sie einen Wert aus dem-Steuerelement empfängt, und legen Sie den Namen des-Parameters auf den Namen des Steuer Elements fest, das den Wert bereitstellt.

Um das Steuerelement Attribut aufzulösen, müssen Sie eine **using** -Anweisung für den **System. Web. modelbinding** -Namespace hinzufügen.

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample4.cs)]

Der folgende Code zeigt die ausgewählte Methode zum Filtern der zurückgegebenen Daten basierend auf dem Wert der Dropdown Liste. Durch das Hinzufügen eines Steuerelement Attributs vor einem Parameter wird angegeben, dass der Wert für diesen Parameter von einem Steuerelement mit dem gleichen Namen stammt.

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample5.cs)]

Führen Sie die Webanwendung aus, und wählen Sie in der Dropdown Liste verschiedene Werte aus, um die Liste der Studenten zu filtern.

![Schüler/Studenten Filtern](sorting-paging-and-filtering-data/_static/image6.png)

## <a name="conclusion"></a>Zusammenfassung

In diesem Tutorial haben Sie das Sortieren und Paging der Daten aktiviert. Sie haben auch das Filtern der Daten nach dem Wert eines Steuer Elements aktiviert.

Im nächsten [Tutorial](integrating-jquery-ui.md) verbessern Sie die Benutzeroberfläche, indem Sie ein jQuery UI-Widget in die Vorlage für dynamische Daten integrieren.

> [!div class="step-by-step"]
> [Zurück](updating-deleting-and-creating-data.md)
> [Weiter](integrating-jquery-ui.md)
