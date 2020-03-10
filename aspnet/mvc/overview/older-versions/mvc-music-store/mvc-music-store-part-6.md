---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
title: 'Teil 6: Verwenden von Daten Anmerkungen für die Modell Validierung | Microsoft-Dokumentation'
author: jongalloway
description: In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der ASP.NET MVC Music Store-Beispielanwendung ausgeführt wurden. Teil 6 umfasst die Verwendung von Daten Anmerkungen für Modell V...
ms.author: riande
ms.date: 04/21/2011
ms.assetid: b3193d33-2d0b-4d98-9712-58bd897c62ec
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
msc.type: authoredcontent
ms.openlocfilehash: bc031dd5be61cc6707c522f85f6af77a420c8b31
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78433533"
---
# <a name="part-6-using-data-annotations-for-model-validation"></a>Teil 6: Verwenden von Daten Anmerkungen für die Modell Validierung

von [Jon Galloway](https://github.com/jongalloway)

> Der MVC Music Store ist eine Lernprogramm Anwendung, die Schritt für Schritt erläutert, wie ASP.NET MVC und Visual Studio für die Webentwicklung verwendet werden.  
>   
> Der MVC Music Store ist eine einfache Beispiel Speicher Implementierung, die Musikalben online verkauft und grundlegende Funktionen für Website Verwaltung, Benutzeranmeldung und Warenkorb implementiert.  
>   
> In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der ASP.NET MVC Music Store-Beispielanwendung ausgeführt wurden. In Teil 6 wird die Verwendung von Daten Anmerkungen für die Modell Validierung behandelt.

Das Erstellungs-und Bearbeitungs Formular hat ein großes Problem: Sie führen keine Validierung durch. Wir können z. b. die Pflichtfelder leer lassen oder Buchstaben im Feld Price eingeben. der erste Fehler, den wir sehen werden, ist die Datenbank.

Wir können unserer Anwendung problemlos eine Validierung hinzufügen, indem wir unseren Modellklassen Daten Anmerkungen hinzufügen. Mit Daten Anmerkungen können wir die Regeln beschreiben, die auf unsere Modell Eigenschaften angewendet werden sollen, und ASP.NET MVC übernimmt die Erzwingung und zeigt die entsprechenden Nachrichten für unsere Benutzer an.

## <a name="adding-validation-to-our-album-forms"></a>Hinzufügen von Validierungen zu unseren Album Formularen

Wir verwenden die folgenden Daten Anmerkungen-Attribute:

- **Required** – gibt an, dass die Eigenschaft ein Pflichtfeld ist.
- **Display Name** – definiert den Text, der für Formularfelder und Validierungs Nachrichten verwendet werden soll.
- **StringLength** – definiert eine maximale Länge für ein Zeichen folgen Feld.
- **Range** – gibt einen maximalen und minimalen Wert für ein numerisches Feld an.
- **Bind** – listet Felder auf, die beim Binden von Parameter-oder Formular Werten an Modell Eigenschaften ausgeschlossen oder eingeschlossen werden sollen.
- **Gerüst Column** – ermöglicht das Ausblenden von Feldern aus Editor Formularen

*Hinweis: Weitere Informationen zur Modell Validierung mithilfe von Attributen für die Daten Anmerkung finden Sie in der MSDN-Dokumentation unter* [`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)

Öffnen Sie die Album-Klasse, und fügen Sie die folgenden *using* -Anweisungen am Anfang hinzu.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample1.cs)]

Aktualisieren Sie anschließend die Eigenschaften, um die Anzeige-und Validierungs Attribute hinzuzufügen, wie unten gezeigt.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample2.cs)]

Während wir dort sind, haben wir auch das Genre und den Künstler in Virtual Properties geändert. Dadurch können Entity Framework diese ggf. verzögert laden.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample3.cs)]

Nachdem diese Attribute dem Album Modell hinzugefügt wurden, beginnt der Bildschirm zum Erstellen und bearbeiten sofort mit der Überprüfung der Felder und der Verwendung der ausgewählten anzeigen Amen (z. b. "Album Kunst-URL" anstelle von "albumarturl"). Führen Sie die Anwendung aus, und navigieren Sie zu/StoreManager/Create.

![](mvc-music-store-part-6/_static/image1.png)

Als nächstes unterbrechen wir einige Validierungsregeln. Geben Sie einen Preis von 0 ein, und lassen Sie den Titel leer. Wenn Sie auf die Schaltfläche "erstellen" klicken, wird das Formular mit Validierungs Fehlermeldungen angezeigt, das anzeigt, welche Felder die von uns definierten Validierungsregeln nicht erfüllen.

![](mvc-music-store-part-6/_static/image2.png)

## <a name="testing-the-client-side-validation"></a>Testen der Client seitigen Validierung

Die Server seitige Validierung ist aus Anwendungs Sicht sehr wichtig, da Benutzer die Client seitige Validierung umgehen können. Webseiten Formulare, die nur serverseitige Validierung implementieren, weisen jedoch drei bedeutende Probleme auf.

1. Der Benutzer muss warten, bis das Formular gepostet, auf dem Server überprüft und die Antwort an den Browser gesendet wird.
2. Der Benutzer erhält kein sofortiges Feedback, wenn er ein Feld korrigiert, sodass es nun die Validierungsregeln übergibt.
3. Wir verschwenden Server Ressourcen, um Validierungs Logik auszuführen, anstatt den Browser des Benutzers zu nutzen.

Glücklicherweise ist für die ASP.NET MVC 3-Gerüst Vorlagen die Client seitige Validierung integriert, sodass keine zusätzliche Arbeit erforderlich ist.

Wenn Sie einen einzelnen Buchstaben im Feld "Title" eingeben, werden die Überprüfungsanforderungen erfüllt, sodass die Validierungs Meldung sofort entfernt wird.

![](mvc-music-store-part-6/_static/image3.png)

> [!div class="step-by-step"]
> [Zurück](mvc-music-store-part-5.md)
> [Weiter](mvc-music-store-part-7.md)
