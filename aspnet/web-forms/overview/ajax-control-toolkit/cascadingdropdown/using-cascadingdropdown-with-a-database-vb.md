---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-vb
title: Verwenden von CascadingDropDown mit einer Datenbank (VB) | Microsoft-Dokumentation
author: wenz
description: Das CascadingDropDown-Steuerelement im AJAX Control Toolkit erweitert ein DropDownList-Steuerelement, sodass Änderungen in einer DropDownList zugeordnete Werte in Anoth laden...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 97a3d33c-c856-43f3-8acb-f1ccbc48221a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-vb
msc.type: authoredcontent
ms.openlocfilehash: 4482aa18c4446ec8f5f160c423008398ea2e1d0d
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599464"
---
# <a name="using-cascadingdropdown-with-a-database-vb"></a>Verwenden von CascadingDropDown mit einer Datenbank (VB)

von [Christian Wenz](https://github.com/wenz)

[Code herunterladen](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown1.vb.zip) oder [PDF herunterladen](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown1VB.pdf)

> Das CascadingDropDown-Steuerelement im AJAX Control Toolkit erweitert ein DropDownList-Steuerelement, sodass Änderungen in einer Dropdown List zugeordnete Werte in eine andere Dropdown List laden. Damit dies funktioniert, muss ein spezieller Webdienst erstellt werden.

## <a name="overview"></a>Übersicht über

Das CascadingDropDown-Steuerelement im AJAX Control Toolkit erweitert ein DropDownList-Steuerelement, sodass Änderungen in einer Dropdown List zugeordnete Werte in eine andere Dropdown List laden. (Eine Liste enthält beispielsweise eine Liste der US-Bundesstaaten, und die nächste Liste wird dann mit den wichtigsten Städten in diesem Zustand aufgefüllt.) Damit dies funktioniert, muss ein spezieller Webdienst erstellt werden.

## <a name="steps"></a>Schritte

Zuerst ist eine Datenquelle erforderlich. In diesem Beispiel werden die AdventureWorks-Datenbank und die Microsoft SQL Server 2005 Express Edition verwendet. Die Datenbank ist ein optionaler Teil einer Visual Studio-Installation (einschließlich Express Edition) und ist auch als separater Download unter [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064)verfügbar. Die AdventureWorks-Datenbank ist Teil der SQL Server 2005-Beispiele und-Beispiel Datenbanken (Download bei [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;D isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Die einfachste Möglichkeit, die Datenbank einzurichten, ist die Verwendung des Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;D isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) und das Anfügen der `AdventureWorks.mdf` Datenbankdatei.

Bei diesem Beispiel wird davon ausgegangen, dass die Instanz des SQL Server 2005 Express Edition `SQLEXPRESS` ist und sich auf demselben Computer wie der Webserver befindet. Dies ist auch das Standard Setup. Wenn das Setup abweicht, müssen Sie die Verbindungsinformationen für die Datenbank anpassen.

Um die Funktionalität von ASP.NET AJAX und dem Steuerelement-Toolkit zu aktivieren, muss das `ScriptManager` Steuerelement an einer beliebigen Stelle auf der Seite platziert werden (innerhalb des &lt;`form`&gt; Element):

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample1.aspx)]

Im nächsten Schritt sind zwei Dropdown List-Steuerelemente erforderlich. In diesem Beispiel verwenden wir die Hersteller-und Kontaktinformationen von AdventureWorks. Daher erstellen wir eine Liste für die verfügbaren Lieferanten und eine für die verfügbaren Kontakte:

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample2.aspx)]

Anschließend müssen der Seite zwei CascadingDropDown-Extender hinzugefügt werden. Eine füllt die erste Liste (Lieferanten) und die andere die zweite Liste (Kontakte) aus. Die folgenden Attribute müssen festgelegt werden:

- `ServicePath`: URL eines Webdiensts, der die Listeneinträge bereitgestellt
- `ServiceMethod`: Webmethode, die die Listeneinträge bereitgestellt
- `TargetControlID`: ID der Dropdown Liste
- `Category`: Kategorieinformationen, die beim Aufrufen an die Webmethode übermittelt werden.
- `PromptText`: Text, der beim asynchronen Laden von Listen Daten vom Server angezeigt wird.
- `ParentControlID`: (optional) übergeordnete Dropdown Liste, die das Laden der aktuellen Liste auslöst

Abhängig von der verwendeten Programmiersprache ändert sich der Name des fraglichen Webdiensts, aber alle anderen Attributwerte sind identisch. Hier ist das CascadingDropDown-Element für die erste Dropdown Liste:

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample3.aspx)]

Die Steuerelement Erweiterungen für die zweite Liste müssen das `ParentControlID`-Attribut festlegen, damit das Auswählen eines Eintrags in der Liste "Lieferanten" das Laden der zugeordneten Elemente in der Liste Kontakte auslöst.

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample4.aspx)]

Die eigentliche Arbeit wird dann im Webdienst durchgeführt, der wie folgt eingerichtet wird. Beachten Sie, dass das `[ScriptService]`-Attribut verwendet wird, andernfalls kann ASP.NET AJAX den JavaScript-Proxy nicht erstellen, um von Client seitigem Skriptcode auf die Webmethoden zuzugreifen.

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample5.aspx)]

Die Signatur der von CascadingDropDown aufgerufenen Webmethoden lautet wie folgt:

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample6.vb)]

Der Rückgabewert muss daher ein Array vom Typ `CascadingDropDownNameValue` sein, das durch das Control Toolkit definiert wird. Die `GetVendors()`-Methode ist recht einfach zu implementieren: der Code stellt eine Verbindung zur AdventureWorks-Datenbank her und fragt die ersten 25 Anbieter ab. Der erste Parameter im `CascadingDropDownNameValue`-Konstruktor ist die Beschriftung des Listen Eintrags, der zweite Parameter ist der Wert (value-Attribut in HTML-&lt;`option`&gt; Element). Hier ist der Code:

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample7.vb)]

Das erhalten der zugeordneten Kontakte für einen Anbieter (Methodenname: `GetContactsForVendor()`) ist etwas schwieriger. Zuerst muss der Hersteller, der in der ersten Dropdown Liste ausgewählt wurde, ermittelt werden. Das Steuerelement-Toolkit definiert eine Hilfsmethode für diese Aufgabe: die `ParseKnownCategoryValuesString()`-Methode gibt ein `StringDictionary`-Element mit den Dropdown Daten zurück:

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample8.vb)]

Aus Sicherheitsgründen müssen diese Daten zuerst überprüft werden. Wenn es also einen Lieferanten Eintrag gibt (da die `Category`-Eigenschaft des ersten CascadingDropDown-Elements auf `"Vendor"`festgelegt ist), kann die ID des ausgewählten Anbieters abgerufen werden:

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample9.vb)]

Der Rest der Methode ist recht einfach, und dann. Die ID des Anbieters wird als Parameter für eine SQL-Abfrage verwendet, die alle zugeordneten Kontakte für diesen Anbieter abruft. Die-Methode gibt wiederum ein Array vom Typ `CascadingDropDownNameValue`zurück.

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample10.vb)]

Laden Sie die Seite ASP.net, und nach kurzer Zeit wird die Anbieterliste mit 25 Einträgen gefüllt. Wählen Sie einen Eintrag aus, und beachten Sie, dass die zweite Dropdown Liste mit Daten gefüllt ist.

[![die erste Liste automatisch ausgefüllt wird](using-cascadingdropdown-with-a-database-vb/_static/image2.png)](using-cascadingdropdown-with-a-database-vb/_static/image1.png)

Die erste Liste wird automatisch ausgefüllt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-cascadingdropdown-with-a-database-vb/_static/image3.png)).

[![die zweite Liste entsprechend der Auswahl in der ersten Liste ausgefüllt ist.](using-cascadingdropdown-with-a-database-vb/_static/image5.png)](using-cascadingdropdown-with-a-database-vb/_static/image4.png)

Die zweite Liste wird entsprechend der Auswahl in der ersten Liste ausgefüllt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-cascadingdropdown-with-a-database-vb/_static/image6.png)).

> [!div class="step-by-step"]
> [Zurück](filling-a-list-using-cascadingdropdown-vb.md)
> [Weiter](presetting-list-entries-with-cascadingdropdown-vb.md)
