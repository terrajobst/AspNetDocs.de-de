---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-vb
title: Dynamisches Auffüllen eines Steuer Elements (VB) | Microsoft-Dokumentation
author: wenz
description: Das DynamicPopulate-Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit Ruft einen Webdienst (oder eine Seiten Methode) auf und füllt den resultierenden Wert in ein Ziel Steuerelement unter t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 27305347-7b5d-4519-97b7-197a357e7f6e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: d11320f1f89bb69afe5f62751574079716124da0
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599398"
---
# <a name="dynamically-populating-a-control-vb"></a>Dynamisches Auffüllen eines Steuerelements (VB)

von [Christian Wenz](https://github.com/wenz)

[Code herunterladen](https://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.vb.zip) oder [PDF herunterladen](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0VB.pdf)

> Das DynamicPopulate-Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit Ruft einen Webdienst (oder eine Seiten Methode) auf und füllt den resultierenden Wert in ein Ziel Steuerelement auf der Seite, ohne eine Seiten Aktualisierung durchführen zu müssen.

## <a name="overview"></a>Übersicht über

Das `DynamicPopulate`-Steuerelement im ASP.NET AJAX Control Toolkit Ruft einen Webdienst (oder eine Seiten Methode) auf und füllt den resultierenden Wert ohne Seiten Aktualisierung in ein Ziel Steuerelement auf der Seite aus. In diesem Tutorial wird gezeigt, wie Sie diese Vorgehensweise einrichten.

## <a name="steps"></a>Schritte

Zunächst benötigen Sie einen ASP.NET-Webdienst, der die Methode implementiert, die von `DynamicPopulate`aufgerufen werden soll. Die Webdienst Klasse erfordert das `ScriptService`-Attribut, das in `Microsoft.Web.Script.Services`definiert ist; Andernfalls kann ASP.NET AJAX den Client seitigen JavaScript-Proxy für den Webdienst nicht erstellen, der wiederum von `DynamicPopulate`benötigt wird.

Die Webmethode muss ein Argument vom Typ "String" mit dem Namen "`contextKey`" erwarten, da das `DynamicPopulate` Steuerelement einen Teil der Kontextinformationen mit jedem Webdienst Aufruf sendet. Der folgende Webdienst gibt das aktuelle Datum in einem Format zurück, das durch das `contextKey`-Argument dargestellt wird:

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample1.aspx)]

Der Webdienst wird dann als `DynamicPopulate.vb.asmx`gespeichert. Alternativ könnten Sie die `getDate()`-Methode als Seiten Methode innerhalb der eigentlichen ASP.NET-Seite mit dem `DynamicPopulate`-Steuerelement implementieren.

Erstellen Sie im nächsten Schritt eine neue ASP.NET-Datei. Wie immer besteht der erste Schritt darin, die `ScriptManager` auf der aktuellen Seite einzuschließen, um die ASP.NET AJAX-Bibliothek zu laden und das steuerungstooltoolkit funktionsfähig zu machen:

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample2.aspx)]

Fügen Sie dann ein Label-Steuerelement hinzu (beispielsweise mit dem HTML-Steuerelement mit dem gleichen Namen oder dem &lt;`asp:Label` /&gt; websteuer Element), das später das Ergebnis des Webdienst Aufrufes anzeigt.

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample3.aspx)]

Eine HTML-Schaltfläche (als HTML-Steuerelement, da kein Postback an den Server erforderlich ist) wird dann verwendet, um die dynamische Auffüllung zu starten:

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample4.aspx)]

Schließlich benötigen wir das `DynamicPopulateExtender`-Steuerelement, um es zu übertragen. Die folgenden Attribute werden festgelegt (außer den offensichtlichen, `ID` und `runat`=`"server"`):

- `TargetControlID`, wo das Ergebnis aus dem Webdienst aufgerufen werden soll.
- `ServicePath` Pfad zum Webdienst (auslassen, wenn Sie eine Seiten Methode verwenden möchten)
- `ServiceMethod` Name der Webmethode oder Seiten Methode
- `ContextKey` Kontextinformationen, die an den Webdienst gesendet werden sollen.
- `PopulateTriggerControlID`-Element, das den Webdienst aufruft.
- `ClearContentsDuringUpdate`, ob das Target-Element während des Webdienst Aufrufes leer ist.

Wie Sie sehen können, benötigt das Steuerelement einige Informationen, aber es ist recht unkompliziert, alles zu platzieren. Im folgenden finden Sie das Markup für das `DynamicPopulateExtender`-Steuerelement im aktuellen Szenario:

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample5.aspx)]

Führen Sie die ASP.NET-Seite im Browser aus, und klicken Sie auf die Schaltfläche. das aktuelle Datum wird im Format Monat-Tag-Jahr angezeigt.

[![ein Klick auf die Schaltfläche ruft das Datum vom Server ab.](dynamically-populating-a-control-vb/_static/image2.png)](dynamically-populating-a-control-vb/_static/image1.png)

Durch Klicken auf die Schaltfläche wird das Datum vom Server abgerufen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](dynamically-populating-a-control-vb/_static/image3.png)).

> [!div class="step-by-step"]
> [Zurück](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)
> [Weiter](dynamically-populating-a-control-using-javascript-code-vb.md)
