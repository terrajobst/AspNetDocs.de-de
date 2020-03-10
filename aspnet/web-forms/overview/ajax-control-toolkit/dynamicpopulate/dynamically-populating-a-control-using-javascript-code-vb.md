---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-vb
title: Dynamisches Auffüllen eines Steuer Elements mithilfe von JavaScript-Code (VB) | Microsoft-Dokumentation
author: wenz
description: Das DynamicPopulate-Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit Ruft einen Webdienst (oder eine Seiten Methode) auf und füllt den resultierenden Wert in ein Ziel Steuerelement unter t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 90582e54-3e90-432a-9da5-689fb39ed56b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-vb
msc.type: authoredcontent
ms.openlocfilehash: b2bd5b1571ccebc9baa501b29743aecdb4543fb2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78497379"
---
# <a name="dynamically-populating-a-control-using-javascript-code-vb"></a>Dynamisches Auffüllen eines Steuerelements über den JavaScript-Code (VB)

von [Christian Wenz](https://github.com/wenz)

[Code herunterladen](https://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.vb.zip) oder [PDF herunterladen](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1VB.pdf)

> Das DynamicPopulate-Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit Ruft einen Webdienst (oder eine Seiten Methode) auf und füllt den resultierenden Wert in ein Ziel Steuerelement auf der Seite, ohne eine Seiten Aktualisierung durchführen zu müssen. Es ist auch möglich, die Auffüllung mithilfe von benutzerdefiniertem Client seitigem JavaScript-Code zu initiieren.

## <a name="overview"></a>Übersicht

Das `DynamicPopulate`-Steuerelement im ASP.NET AJAX Control Toolkit Ruft einen Webdienst (oder eine Seiten Methode) auf und füllt den resultierenden Wert ohne Seiten Aktualisierung in ein Ziel Steuerelement auf der Seite aus. Es ist auch möglich, die Auffüllung mithilfe von benutzerdefiniertem Client seitigem JavaScript-Code zu initiieren.

## <a name="steps"></a>Schritte

Zunächst benötigen Sie einen ASP.NET-Webdienst, der die Methode implementiert, die vom `DynamicPopulateExtender` Steuerelement aufgerufen werden soll. Der Webdienst implementiert die-Methode `getDate()`, die ein Argument vom Typ "String" erwartet, das als `contextKey`bezeichnet wird, da das `DynamicPopulate`-Steuerelement einen Teil der Kontextinformationen mit jedem Webdienst Aufruf sendet. Im folgenden finden Sie den Code (File `DynamicPopulate.vb.asmx`), der das aktuelle Datum in einem von drei Formaten abruft:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample1.aspx)]

Erstellen Sie im nächsten Schritt eine neue ASP.NET-Website, und beginnen Sie mit dem ASP.NET AJAX ScriptManager-Steuerelement:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample2.aspx)]

Fügen Sie dann ein Label-Steuerelement hinzu (beispielsweise mithilfe des HTML-Steuer Elements desselben Namens oder des `<asp:Label />` Web-Steuer Elements), das später das Ergebnis des Webdienst Aufrufes anzeigt.

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample3.aspx)]

Fügen Sie als nächstes ein `DynamicPopulateExtender` Steuerelement hinzu, und geben Sie Webdienst Informationen, das Ziel Steuerelement, aber nicht den Namen des Steuer Elements an, das die Auffüllung auslöst. Dies wird später mithilfe von benutzerdefiniertem JavaScript durchgeführt.

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample4.aspx)]

Nun zum JavaScript-Teil. Die `$find()` Funktion, die von der ASP.NET AJAX-Bibliothek definiert wird, gibt einen Verweis auf serverseitige Objekte des ASP.NET AJAX Control Toolkit zurück, z. b. `DynamicPopulateExtender`. In der aktuellen Datei gibt `$find("dpe")` einen Verweis auf den `DynamicPopulateExtender` Steuerelement auf der Seite zurück. Sie macht eine Methode namens `populate()` verfügbar, die den dynamischen auffüllungs Prozess auslöst. Die `populate()`-Methode erfordert ein Argument: den Kontext Schlüssel, der als Argument für die `getDate()` Webmethode fungiert. Beispielsweise wird `$find("dpe").populate("format1")` die Bezeichnung mit dem aktuellen Datum im Format month-day-year auffüllen.

Damit das Beispiel etwas flexibler wird, kann der Benutzer jetzt zwischen mehreren Datumsformaten wählen. Für jeden dieser Elemente wird ein Optionsfeld angezeigt. Sobald der Benutzer auf ein Optionsfeld klickt, füllt JavaScript-Code die Bezeichnung dynamisch mit dem ausgewählten Datumsformat auf. Dies sind die folgenden Options Felder:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample5.aspx)]

Beachten Sie, dass sich der JavaScript-`this.value` Ausdruck im Kontext eines Options Felds auf den Wert der aktuellen Schaltfläche bezieht, bei der es sich um genau dieselben Informationen handelt, mit denen die `getDate()` Methode funktionieren kann.

[![ein Klick auf die Schaltfläche ruft das Datum vom Server im angegebenen Format ab.](dynamically-populating-a-control-using-javascript-code-vb/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-vb/_static/image1.png)

Wenn Sie auf die Schaltfläche klicken, wird das Datum vom Server im angegebenen Format abgerufen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](dynamically-populating-a-control-using-javascript-code-vb/_static/image3.png)).

> [!div class="step-by-step"]
> [Zurück](dynamically-populating-a-control-vb.md)
> [Weiter](using-dynamicpopulate-with-a-user-control-and-javascript-vb.md)
