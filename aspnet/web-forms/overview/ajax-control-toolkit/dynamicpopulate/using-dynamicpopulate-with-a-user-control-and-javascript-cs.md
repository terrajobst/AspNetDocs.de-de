---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
title: Verwenden von DynamicPopulate mit einem Benutzer Steuerelement undC#JavaScript () | Microsoft-Dokumentation
author: wenz
description: Das DynamicPopulate-Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit Ruft einen Webdienst (oder eine Seiten Methode) auf und füllt den resultierenden Wert in ein Ziel Steuerelement unter t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 38ac8250-8854-444c-b9ab-8998faa41c5a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: a0e6d04a5f62ab558aceb8302d94d3bf2dc8a39f
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599165"
---
# <a name="using-dynamicpopulate-with-a-user-control-and-javascript-c"></a>Verwenden von DynamicPopulate mit einem Benutzersteuerelement und JavaScript (C#)

von [Christian Wenz](https://github.com/wenz)

[Code herunterladen](https://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.cs.zip) oder [PDF herunterladen](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2CS.pdf)

> Das DynamicPopulate-Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit Ruft einen Webdienst (oder eine Seiten Methode) auf und füllt den resultierenden Wert in ein Ziel Steuerelement auf der Seite, ohne eine Seiten Aktualisierung durchführen zu müssen. Es ist auch möglich, die Auffüllung mithilfe von benutzerdefiniertem Client seitigem JavaScript-Code zu initiieren. Es muss jedoch besonders sorgfältig vorgegangen werden, wenn sich der Extender in einem Benutzer Steuerelement befindet.

## <a name="overview"></a>Übersicht über

Das `DynamicPopulate`-Steuerelement im ASP.NET AJAX Control Toolkit Ruft einen Webdienst (oder eine Seiten Methode) auf und füllt den resultierenden Wert ohne Seiten Aktualisierung in ein Ziel Steuerelement auf der Seite aus. Es ist auch möglich, die Auffüllung mithilfe von benutzerdefiniertem Client seitigem JavaScript-Code zu initiieren. Es muss jedoch besonders sorgfältig vorgegangen werden, wenn sich der Extender in einem Benutzer Steuerelement befindet.

## <a name="steps"></a>Schritte

Zunächst benötigen Sie einen ASP.NET-Webdienst, der die Methode implementiert, die vom `DynamicPopulateExtender` Steuerelement aufgerufen werden soll. Der Webdienst implementiert die-Methode `getDate()`, die ein Argument vom Typ "String" erwartet, das als `contextKey`bezeichnet wird, da das `DynamicPopulate`-Steuerelement einen Teil der Kontextinformationen mit jedem Webdienst Aufruf sendet. Im folgenden finden Sie den Code (File `DynamicPopulate.cs.asmx`), der das aktuelle Datum in einem von drei Formaten abruft:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample1.aspx)]

Erstellen Sie im nächsten Schritt ein neues Benutzer Steuerelement (`.ascx` Datei), das in der ersten Zeile mit der folgenden Deklaration gekennzeichnet ist:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample2.aspx)]

Ein &lt;`label`&gt;-Element wird verwendet, um die vom Server kommenden Daten anzuzeigen.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample3.aspx)]

Außerdem verwenden wir in der Benutzer Steuerelement-Datei drei Options Felder, die jeweils eines der drei möglichen Datumsformate darstellen, die vom Webdienst unterstützt werden. Wenn der Benutzer auf eine der Options Felder klickt, führt der Browser JavaScript-Code aus, der wie folgt aussieht:

[!code-powershell[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample4.ps1)]

Dieser Code greift auf die `DynamicPopulateExtender` zu (machen Sie sich noch keine Gedanken über die seltsame ID, diese wird später behandelt) und löst die dynamische Auffüllung mit Daten aus. Im Kontext des aktuellen Options Felds verweist `this.value` auf seinen Wert, der `format1`, `format2` oder `format3` genau der von der Webmethode erwarteten Wert ist.

Das einzige, was im Benutzer Steuerelement fehlt, ist das `DynamicPopulateExtender` Steuerelement, mit dem die Options Felder mit dem Webdienst verknüpft werden.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample5.aspx)]

Auch hier können Sie die seltsame ID beachten, die im Steuerelement verwendet wird: `mcd1$myDate` statt `myDate`. Zuvor hat der JavaScript-Code, der für den Zugriff auf die `DynamicPopulateExtender` verwendet wurde, anstelle von `dpe1``mcd1_dpe1`. Diese Benennungs Strategie ist eine besondere Anforderung, wenn Sie `DynamicPopulateExtender` innerhalb eines Benutzer Steuer Elements verwenden. Außerdem müssen Sie das Benutzer Steuerelement auf eine bestimmte Weise einbetten, damit es alles funktioniert. Erstellen Sie eine neue ASP.NET-Seite, und registrieren Sie ein Tagpräfix für das Benutzer Steuerelement, das Sie soeben implementiert haben:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample6.aspx)]

Fügen Sie dann das ASP.NET AJAX `ScriptManager`-Steuerelement auf der neuen Seite hinzu:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample7.aspx)]

Fügen Sie abschließend der Seite das Benutzer Steuerelement hinzu. Sie müssen lediglich das `ID` Attribut festlegen (und natürlich `runat="server"`), aber Sie müssen es auch auf einen bestimmten Namen festlegen: `mcd1`, da dies das Präfix ist, das innerhalb des Benutzer Steuer Elements verwendet wird, um über JavaScript darauf zuzugreifen.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample8.aspx)]

Und das ist schon alles! Die Seite verhält sich erwartungsgemäß: ein Benutzer klickt auf eines der Options Felder, das Steuerelement im Toolkit Ruft den Webdienst auf und zeigt das aktuelle Datum im gewünschten Format an.

[![sich die Options Felder in einem Benutzer Steuerelement befinden.](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image1.png)

Die Options Felder befinden sich in einem Benutzer Steuerelement ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Zurück](dynamically-populating-a-control-using-javascript-code-cs.md)
> [Weiter](dynamically-populating-a-control-vb.md)
