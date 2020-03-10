---
uid: web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
title: Hinzufügen einer Geschäftslogik Ebene zu einem Projekt, das Modell Bindung und Web Forms verwendet | Microsoft-Dokumentation
author: Rick-Anderson
description: In dieser tutorialreihe werden grundlegende Aspekte der Verwendung der Modell Bindung mit einem ASP.net Web Forms-Projekt veranschaulicht. Die Modell Bindung führt zu einer geraden Daten Interaktion-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 7ef664b3-1cc8-4cbf-bb18-9f0f3a3ada2b
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
msc.type: authoredcontent
ms.openlocfilehash: a824d06d3781e11706f2a48d44ea3ad89bdb7c8b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78515427"
---
# <a name="adding-business-logic-layer-to-a-project-that-uses-model-binding-and-web-forms"></a>Hinzufügen einer Geschäftslogik Ebene zu einem Projekt, das Modell Bindung und Web Forms verwendet

von [Tom fitzmacken](https://github.com/tfitzmac)

> In dieser tutorialreihe werden grundlegende Aspekte der Verwendung der Modell Bindung mit einem ASP.net Web Forms-Projekt veranschaulicht. Die Modell Bindung sorgt für eine genauere Daten Interaktion als bei der Verarbeitung von Datenquellen Objekten (z. b. ObjectDataSource oder SqlDataSource). Diese Serie beginnt mit Einführungs Material und wechselt in spätere Tutorials zu erweiterten Konzepten.
> 
> In diesem Tutorial wird gezeigt, wie die Modell Bindung mit einer Geschäftslogik Schicht verwendet wird. Sie legen den oncallingdatamethods-Member fest, um anzugeben, dass ein anderes Objekt als die aktuelle Seite verwendet wird, um die Daten Methoden aufzurufen.
> 
> Dieses Tutorial baut auf dem Projekt auf, das in den [vorherigen](retrieving-data.md) Teilen der Reihe erstellt wurde.
> 
> Sie können das gesamte Projekt in C# oder VB [herunterladen](https://go.microsoft.com/fwlink/?LinkId=286116) . Der herunterladbare Code funktioniert entweder mit Visual Studio 2012 oder Visual Studio 2013. Dabei wird die Vorlage Visual Studio 2012 verwendet, die sich geringfügig von der in diesem Tutorial gezeigten Visual Studio 2013 Vorlage unterscheidet.

## <a name="what-youll-build"></a>Was Sie erstellen

Mithilfe der Modell Bindung können Sie Ihren Daten Interaktions Code entweder in der Code Behind-Datei für eine Webseite oder in einer separaten Geschäftslogik Klasse platzieren. In den vorherigen Tutorials wurde gezeigt, wie die Code-Behind-Dateien für den Daten Interaktions Code verwendet werden. Diese Vorgehensweise funktioniert bei kleinen Standorten, kann jedoch zu einer Code Wiederholung und einer größeren Schwierigkeit bei der Verwaltung einer großen Website führen. Es kann auch sehr schwierig sein, Code Programm gesteuert zu testen, der sich in Code Behind-Dateien befindet, da keine Abstraktions Ebene vorhanden ist.

Um den Daten Interaktions Code zu zentralisieren, können Sie eine Geschäftslogik Ebene erstellen, die die gesamte Logik für die Interaktion mit Daten enthält. Anschließend wird die Geschäftslogik Schicht auf Ihren Webseiten aufgerufen. In diesem Tutorial wird gezeigt, wie Sie den gesamten Code, den Sie in den vorherigen Tutorials geschrieben haben, in eine Geschäftslogik Schicht verschieben und diesen Code dann auf den Seiten verwenden können.

In diesem Tutorial gehen Sie wie folgt vor:

1. Verschieben Sie den Code aus Code-Behind-Dateien in eine Geschäftslogik Ebene.
2. Ändern Sie die Daten gebundenen Steuerelemente, um die Methoden in der Geschäftslogik Schicht aufzurufen.

## <a name="create-business-logic-layer"></a>Geschäftslogik Ebene erstellen

Nun erstellen Sie die-Klasse, die von den Webseiten aufgerufen wird. Die Methoden in dieser Klasse ähneln den Methoden, die Sie in den vorherigen Tutorials verwendet haben, und enthalten die Wert Anbieter Attribute.

Fügen Sie zunächst einen neuen Ordner namens **BLL**hinzu.

![Ordner hinzufügen](adding-business-logic-layer/_static/image1.png)

Erstellen Sie im Ordner BLL eine neue Klasse mit dem Namen **SchoolBL.cs**. Sie enthält alle Daten Vorgänge, deren Größe sich in Code Behind-Dateien ursprünglich geändert hat. Die Methoden sind fast identisch mit den Methoden in der Code-Behind-Datei, enthalten jedoch einige Änderungen.

Die wichtigste Änderung ist, dass Sie den Code nicht mehr in einer Instanz der **Page** -Klasse ausführen. Die Page-Klasse enthält die **tryupdatemodel** -Methode und die **modelstate** -Eigenschaft. Wenn dieser Code in eine Geschäftslogik Schicht verschoben wird, verfügen Sie nicht mehr über eine Instanz der Page-Klasse, um diese Member aufzurufen. Um dieses Problem zu umgehen, müssen Sie jeder Methode, die auf tryupdatemodel oder modelstate zugreift, einen **modelmethodcontext** -Parameter hinzufügen. Sie verwenden diesen modelmethodcontext-Parameter zum Aufrufen von tryupdatemodel oder zum Abrufen von modelstate. Sie müssen nichts auf der Webseite ändern, um diesen neuen Parameter zu berücksichtigen.

Ersetzen Sie den Code in SchoolBL.cs durch den folgenden Code.

[!code-csharp[Main](adding-business-logic-layer/samples/sample1.cs)]

## <a name="revise-existing-pages-to-retrieve-data-from-business-logic-layer"></a>Überarbeiten vorhandener Seiten zum Abrufen von Daten aus der Geschäftslogik Schicht

Schließlich konvertieren Sie die Seiten "students. aspx", "addstudent. aspx" und "Courses. aspx" aus der Verwendung von Abfragen in der Code-Behind-Datei in die Geschäftslogik Schicht.

Löschen Sie in den Code Behind-Dateien für Studenten, addstudent und Kurse die folgenden Abfrage Methoden, oder kommentieren Sie Sie aus:

- studentsgrid\_GetData
- studentsgrid\_UpdateItem
- studentsgrid\_DeleteItem
- addstudentform\_InsertItem
- coursesgrid\_GetData

Sie sollten jetzt keinen Code in der Code Behind-Datei haben, die sich auf Daten Vorgänge bezieht.

Der **oncallingdatamethods** -Ereignishandler ermöglicht es Ihnen, ein Objekt anzugeben, das für die Daten Methoden verwendet werden soll. Fügen Sie in students. aspx einen Wert für diesen Ereignishandler hinzu, und ändern Sie die Namen der Daten Methoden in die Namen der Methoden in der Geschäftslogik Klasse.

[!code-aspx[Main](adding-business-logic-layer/samples/sample2.aspx?highlight=3-4,8)]

Definieren Sie in der Code Behind-Datei für students. aspx den Ereignishandler für das Ereignis callingdatamethods. In diesem Ereignishandler geben Sie die Geschäftslogik Klasse für Daten Vorgänge an.

[!code-csharp[Main](adding-business-logic-layer/samples/sample3.cs)]

Nehmen Sie in addstudent. aspx ähnliche Änderungen vor.

[!code-aspx[Main](adding-business-logic-layer/samples/sample4.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample5.cs)]

Nehmen Sie in "Courses. aspx" ähnliche Änderungen vor.

[!code-aspx[Main](adding-business-logic-layer/samples/sample6.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample7.cs)]

Führen Sie die Anwendung aus, und beachten Sie, dass alle Seiten wie zuvor funktionieren. Die Validierungs Logik funktioniert ebenfalls ordnungsgemäß.

## <a name="conclusion"></a>Zusammenfassung

In diesem Tutorial haben Sie die Anwendung so strukturiert, dass Sie eine Datenzugriffs Schicht und eine Geschäftslogik Schicht verwendet. Sie haben angegeben, dass die Daten Steuerelemente ein Objekt verwenden, das nicht die aktuelle Seite für Daten Vorgänge ist.

> [!div class="step-by-step"]
> [Previous](using-query-string-values-to-retrieve-data.md)
