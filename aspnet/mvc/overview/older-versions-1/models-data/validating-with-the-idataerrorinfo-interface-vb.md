---
uid: mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-vb
title: Validieren mit der IDataErrorInfo-Schnittstelle (VB) | Microsoft-Dokumentation
author: StephenWalther
description: Stephen Walther zeigt Ihnen, wie Sie benutzerdefinierte Validierungs Fehlermeldungen anzeigen, indem Sie die IDataErrorInfo-Schnittstelle in einer Modell Klasse implementieren.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 3a8a9d9f-82dd-4959-b7c6-960e9ce95df1
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 8ff3adc5db8114dcca2c66d937e1706f0bac0d30
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78436299"
---
# <a name="validating-with-the-idataerrorinfo-interface-vb"></a>Überprüfen mit der IDataErrorInfo-Schnittstelle (VB)

von [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther zeigt Ihnen, wie Sie benutzerdefinierte Validierungs Fehlermeldungen anzeigen, indem Sie die IDataErrorInfo-Schnittstelle in einer Modell Klasse implementieren.

In diesem Tutorial wird erläutert, wie Sie eine Validierung in einer ASP.NET MVC-Anwendung ausführen. Sie erfahren, wie Sie verhindern können, dass jemand ein HTML-Formular sendet, ohne für erforderliche Formularfelder Werte bereitzustellen. In diesem Tutorial erfahren Sie, wie Sie die Überprüfung mithilfe der ierrordatainfo-Schnittstelle ausführen.

## <a name="assumptions"></a>Annahmen

In diesem Tutorial verwende ich die Datenbank "moviesdb" und die "Movies"-Datenbanktabelle. Diese Tabelle weist die folgenden Spalten auf:

<a id="0.6_table01"></a>

| **Spaltenname** | **Datentyp** | **NULL-Werte zulassen** |
| --- | --- | --- |
| Id | Int | False |
| Titel | Nvarchar (100) | False |
| Regisseur | Nvarchar (100) | False |
| Datereleasing | DateTime | False |

In diesem Tutorial verwende ich die Microsoft-Entity Framework, um meine Datenbankmodell Klassen zu generieren. Die vom Entity Framework generierte Movie-Klasse wird in Abbildung 1 angezeigt.

[![der Movie-Entität](validating-with-the-idataerrorinfo-interface-vb/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image1.png)

**Abbildung 01**: die Film Entität ([Klicken Sie, um das Bild in voller Größe anzuzeigen](validating-with-the-idataerrorinfo-interface-vb/_static/image2.png))

> [!NOTE] 
> 
> Weitere Informationen zum Verwenden der Entity Framework zum Generieren von Datenbankmodell Klassen finden Sie in meinem Tutorial zum Erstellen von Modellklassen mit dem Entity Framework.

## <a name="the-controller-class"></a>Die Controller Klasse

Wir verwenden den Home-Controller zum Auflisten von Filmen und zum Erstellen neuer Filme. Der Code für diese Klasse ist in der Liste 1 enthalten.

**Codebeispiel 1-controllers\homecontroller.vb**

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample1.vb)]

Die Home Controller-Klasse in der Liste 1 enthält zwei Create ()-Aktionen. Die erste Aktion zeigt das HTML-Formular zum Erstellen eines neuen Films an. Die zweite Create ()-Aktion führt den eigentlichen Einfügevorgang des neuen Films in der Datenbank aus. Die zweite Create ()-Aktion wird aufgerufen, wenn das Formular, das von der ersten Create ()-Aktion angezeigt wird, an den Server übermittelt wird.

Beachten Sie, dass die zweite Create ()-Aktion die folgenden Codezeilen enthält:

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample2.vb)]

Die IsValid-Eigenschaft gibt false zurück, wenn ein Validierungs Fehler vorliegt. In diesem Fall wird die CREATE VIEW, die das HTML-Formular zum Erstellen eines Films enthält, erneut angezeigt.

## <a name="creating-a-partial-class"></a>Erstellen einer partiellen Klasse

Die Movie-Klasse wird vom Entity Framework generiert. Sie können den Code für die Movie-Klasse sehen, wenn Sie die Datei "moviesdbmodel. edmx" im Projektmappen-Explorer Fenster erweitern und die Datei "moviesdbmodel. Designer. vb" im Code-Editor öffnen (siehe Abbildung 2).

[![den Code für die Movie-Entität](validating-with-the-idataerrorinfo-interface-vb/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image3.png)

**Abbildung 02**: der Code für die Movie-Entität ([Klicken Sie, um das Bild in voller Größe anzuzeigen](validating-with-the-idataerrorinfo-interface-vb/_static/image4.png))

Die Movie-Klasse ist eine partielle Klasse. Dies bedeutet, dass wir eine weitere partielle Klasse mit dem gleichen Namen hinzufügen können, um die Funktionalität der Movie-Klasse zu erweitern. Wir fügen die Validierungs Logik der neuen partiellen Klasse hinzu.

Fügen Sie die-Klasse in der Liste 2 dem Ordner Models hinzu.

**Codebeispiel 2: models\muvie.vb**

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample3.vb)]

Beachten Sie, dass die-Klasse in der Liste 2 den *partiellen* Modifizierer enthält. Alle Methoden oder Eigenschaften, die Sie dieser Klasse hinzufügen, werden Teil der vom Entity Framework generierten Movie-Klasse.

## <a name="adding-onchanging-and-onchanged-partial-methods"></a>Hinzufügen von partiellen und OnChanged-partiellen Methoden

Wenn die Entity Framework eine Entitäts Klasse generiert, fügt die Entity Framework der Klasse automatisch partielle Methoden hinzu. Der Entity Framework generiert partielle und OnChanged-partielle Methoden, die den einzelnen Eigenschaften der-Klasse entsprechen.

Im Fall der Movie-Klasse erstellt der Entity Framework die folgenden Methoden:

- OnIdChanging
- OnIdChanged
- OnTitleChanging
- OnTitleChanged
- OnDirectorChanging
- OnDirectorChanged
- OnDateReleasedChanging
- OnDateReleasedChanged

Die onchanging-Methode wird aufgerufen, unmittelbar bevor die entsprechende-Eigenschaft geändert wird. Die OnChanged-Methode wird aufgerufen, unmittelbar nachdem die-Eigenschaft geändert wurde.

Sie können diese partiellen Methoden nutzen, um der Movie-Klasse Validierungs Logik hinzuzufügen. Die Update Movie-Klasse in der Liste 3 überprüft, ob den Titel-und Director-Eigenschaften nicht leere Werte zugewiesen werden.

> [!NOTE] 
> 
> Bei einer partiellen Methode handelt es sich um eine Methode, die in einer Klasse definiert ist, die nicht implementiert werden muss. Wenn Sie keine partielle Methode implementieren, entfernt der Compiler die Methoden Signatur und alle Aufrufe der-Methode, sodass der partiellen Methode keine Lauf Zeit Kosten zugeordnet sind. Im Visual Studio Code-Editor können Sie eine partielle Methode hinzufügen, indem Sie das Schlüsselwort *partiell* eingeben, gefolgt von einem Leerzeichen, um eine Liste der zu implementierenden partitionale anzuzeigen.

**Codebeispiel 3: models\muvie.vb**

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample4.vb)]

Wenn Sie z. b. versuchen, der Title-Eigenschaft eine leere Zeichenfolge zuzuweisen, wird eine Fehlermeldung einem Wörterbuch mit dem Namen \_Fehlern zugewiesen.

An diesem Punkt geschieht nichts tatsächlich, wenn Sie der Title-Eigenschaft eine leere Zeichenfolge zuweisen und dem Feld "private \_Fehler" ein Fehler hinzugefügt wird. Wir müssen die IDataErrorInfo-Schnittstelle implementieren, um diese Validierungs Fehler für das ASP.NET MVC-Framework verfügbar zu machen.

## <a name="implementing-the-idataerrorinfo-interface"></a>Implementieren der IDataErrorInfo-Schnittstelle

Die IDataErrorInfo-Schnittstelle ist seit der ersten Version Bestandteil von .NET Framework. Diese Schnittstelle ist eine sehr einfache Schnittstelle:

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample5.vb)]

Wenn eine Klasse die IDataErrorInfo-Schnittstelle implementiert, verwendet das ASP.NET-MVC-Framework diese Schnittstelle, wenn eine Instanz der-Klasse erstellt wird. Die "Home Controller Create ()"-Aktion akzeptiert z. b. eine Instanz der Movie-Klasse:

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample6.vb)]

Das ASP.NET-MVC-Framework erstellt die Instanz des Films, das an die Create ()-Aktion über einen Modell Binder (DefaultModelBinder) übertragen wird. Der Modell Binder ist verantwortlich für das Erstellen einer Instanz des Movie-Objekts, indem die HTML-Formularfelder an eine Instanz des Movie-Objekts gebunden werden.

Der DefaultModelBinder erkennt, ob eine Klasse die IDataErrorInfo-Schnittstelle implementiert. Wenn eine Klasse diese Schnittstelle implementiert, ruft der Modell Binder IDataErrorInfo. this Indexer für jede Eigenschaft der Klasse auf. Wenn der Indexer eine Fehlermeldung zurückgibt, wird diese Fehlermeldung vom Modell Binder automatisch dem Modell Status hinzugefügt.

Der DefaultModelBinder überprüft auch die IDataErrorInfo. Error-Eigenschaft. Diese Eigenschaft ist für die Darstellung nicht Eigenschafts spezifischer Validierungs Fehler vorgesehen, die der-Klasse zugeordnet sind. Beispielsweise möchten Sie möglicherweise eine Validierungs Regel erzwingen, die von den Werten mehrerer Eigenschaften der Movie-Klasse abhängt. In diesem Fall würden Sie einen Validierungs Fehler von der Error-Eigenschaft zurückgeben.

Die aktualisierte Movie-Klasse in der Liste 4 implementiert die IDataErrorInfo-Schnittstelle.

**Codebeispiel 4: models\muvie.vb (implementiert IDataErrorInfo)**

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample7.vb)]

In der Auflistung 4 prüft die Indexer-Eigenschaft die \_Errors-Auflistung, um festzustellen, ob Sie einen Schlüssel enthält, der dem an den Indexer übergebenen Eigenschaftsnamen entspricht. Wenn der-Eigenschaft kein Validierungs Fehler zugeordnet ist, wird eine leere Zeichenfolge zurückgegeben.

Sie müssen den Home Controller in keiner Weise ändern, um die geänderte Movie-Klasse zu verwenden. Die in Abbildung 3 gezeigte Seite zeigt, was geschieht, wenn kein Wert für die Formularfelder "Titel" oder "Director" eingegeben wird.

[Automatisches Erstellen von Aktionsmethoden ![](validating-with-the-idataerrorinfo-interface-vb/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image5.png)

**Abbildung 03**: ein Formular mit fehlenden Werten ([Klicken Sie, um das Bild in voller Größe anzuzeigen](validating-with-the-idataerrorinfo-interface-vb/_static/image6.png))

Beachten Sie, dass der datereleasing-Wert automatisch überprüft wird. Da die datereleasing-Eigenschaft keine NULL-Werte akzeptiert, generiert DefaultModelBinder automatisch einen Überprüfungs Fehler für diese Eigenschaft, wenn kein Wert vorhanden ist. Wenn Sie die Fehlermeldung für die datereleasing-Eigenschaft ändern möchten, müssen Sie einen benutzerdefinierten Modell Binder erstellen.

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial haben Sie gelernt, wie Sie die IDataErrorInfo-Schnittstelle verwenden, um Validierungs Fehlermeldungen zu generieren. Zuerst haben wir eine partielle Movie-Klasse erstellt, die die Funktionalität der vom Entity Framework generierten partiellen Movie-Klasse erweitert. Als nächstes haben wir der Methode "OnTitleChanging ()" und "ondirectorchanging ()" eine Validierungs Logik hinzugefügt. Schließlich haben wir die IDataErrorInfo-Schnittstelle implementiert, um diese Validierungs Nachrichten für das ASP.NET MVC-Framework verfügbar zu machen.

> [!div class="step-by-step"]
> [Zurück](performing-simple-validation-vb.md)
> [Weiter](validating-with-a-service-layer-vb.md)
