---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-6
title: Erstellen des JavaScript-Clients | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 20360326-b123-4b1e-abae-1d350edf4ce4
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-6
msc.type: authoredcontent
ms.openlocfilehash: 74f2cc4e5e401d690042b05b028dfc0c46ae282a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78504723"
---
# <a name="create-the-javascript-client"></a>Erstellen des JavaScript-Clients

von [Mike Wasson](https://github.com/MikeWasson)

[Herunterladen des abgeschlossenen Projekts](https://github.com/MikeWasson/BookService)

In diesem Abschnitt erstellen Sie den Client für die Anwendung mithilfe von HTML, JavaScript und der [Knockout. js](http://knockoutjs.com/) -Bibliothek. Die Client-App wird in Phasen erstellt:

- Eine Liste der Bücher wird angezeigt.
- Anzeigen eines Buch Details.
- Hinzufügen eines neuen Buchs.

Die Knockout-Bibliothek verwendet das Model-View-ViewModel (MVVM)-Muster:

- Das **Modell** ist die serverseitige Darstellung der Daten in der Geschäftsdomäne (in unserem Fall Bücher und Autoren).
- Die **Ansicht** ist die Darstellungs Schicht (HTML).
- Das **Ansichts Modell** ist ein JavaScript-Objekt, das die Modelle enthält. Das Ansichts Modell ist eine Code Abstraktion der Benutzeroberfläche. Die HTML-Darstellung ist nicht bekannt. Stattdessen stellt Sie abstrakte Features der Sicht dar, z. b. &quot;eine Liste der Bücher&quot;.

Die Sicht ist Daten gebunden an das Ansichts Modell. Updates für das Ansichts Modell werden automatisch in der Ansicht widergespiegelt. Das Ansichts Modell ruft auch Ereignisse aus der Ansicht ab, z. b. Schaltflächen Klicks.

![](part-6/_static/image1.png)

Mit diesem Ansatz können Sie das Layout und die Benutzeroberfläche Ihrer APP leicht ändern, da Sie die Bindungen ändern können, ohne Code neu schreiben zu müssen. Beispielsweise können Sie eine Liste von Elementen als `<ul>`anzeigen und Sie später in eine Tabelle ändern.

## <a name="add-the-knockout-library"></a>Hinzufügen der Knockout-Bibliothek

Wählen Sie in Visual Studio im **Menü Extras** den Befehl **nuget-Paket-Manager**aus. Klicken Sie anschließend auf **Paket-Manager-Konsole**. Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl ein:

[!code-console[Main](part-6/samples/sample1.cmd)]

Mit diesem Befehl werden die Knockout-Dateien dem Ordner "Scripts" hinzugefügt.

## <a name="create-the-view-model"></a>Erstellen des Ansichts Modells

Fügen Sie dem Ordner Scripts eine JavaScript-Datei mit dem Namen app. js hinzu. (Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf den Ordner Skripts, wählen Sie **Hinzufügen**und dann **JavaScript-Datei**aus.) Fügen Sie den folgenden Code ein:

[!code-javascript[Main](part-6/samples/sample2.js)]

In Knockout ermöglicht die `observable`-Klasse die Datenbindung. Wenn sich der Inhalt einer wahrnehmbaren Änderung ändert, benachrichtigt das Observable-Steuerelement alle Daten gebundenen Steuerelemente, sodass Sie sich selbst aktualisieren können. (Die `observableArray` Klasse ist die Array Version von *Observable*.) Um mit zu beginnen, verfügt unser Ansichts Modell über zwei Observables:

- `books` enthält die Liste der Bücher.
- `error` enthält eine Fehlermeldung, wenn ein AJAX-Befehl fehlschlägt.

Die `getAllBooks`-Methode führt einen AJAX-Befehl aus, um die Liste der Bücher abzurufen. Anschließend wird das Ergebnis auf das `books` Array gepusht.

Die `ko.applyBindings`-Methode ist Teil der Knockout-Bibliothek. Er übernimmt das Ansichts Modell als Parameter und richtet die Datenbindung ein.

## <a name="add-a-script-bundle"></a>Skript Bündel hinzufügen

Bündelung ist ein Feature in ASP.NET 4,5, das das kombinieren oder bündeln mehrerer Dateien in einer einzigen Datei vereinfacht. Die Bündelung reduziert die Anzahl der Anforderungen an den Server, wodurch die Seitenladezeit verbessert werden kann.

Öffnen Sie die Datei-App\_Start/bundleconfig. cs. Fügen Sie der registerbundles-Methode den folgenden Code hinzu.

[!code-csharp[Main](part-6/samples/sample3.cs)]

> [!div class="step-by-step"]
> [Zurück](part-5.md)
> [Weiter](part-7.md)
