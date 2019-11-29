---
uid: web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-vb
title: Verwenden von HoverMenu mit einem Wiederholungs Steuerelement (VB) | Microsoft-Dokumentation
author: wenz
description: 'Das HoverMenu-Steuerelement im AJAX-Steuerelement-Toolkit bietet einen einfachen Popup Effekt: Wenn mit dem Mauszeiger auf ein Element gezeigt wird, wird ein Popup Fenster mit einem bestimmten Element angezeigt...'
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 7f07c112-cd4f-4427-9699-57cfab2791fd
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 9386aa2fe3a6174bbed52218337107733cb1fa99
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606659"
---
# <a name="using-hovermenu-with-a-repeater-control-vb"></a>Verwenden von HoverMenu mit einem Wiederholungssteuerelement (VB)

von [Christian Wenz](https://github.com/wenz)

[Code herunterladen](https://download.microsoft.com/download/b/0/6/b06fe835-5b8f-4c00-aef8-062c19d75b95/HoverMenu1.vb.zip) oder [PDF herunterladen](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/hovermenu1VB.pdf)

> Das HoverMenu-Steuerelement im AJAX Control Toolkit bietet einen einfachen Popup Effekt: Wenn mit dem Mauszeiger auf ein Element gezeigt wird, wird an einer angegebenen Position ein Popup Fenster angezeigt. Es ist auch möglich, dieses Steuerelement in einem Repeater zu verwenden.

## <a name="overview"></a>Übersicht über

Das `HoverMenu`-Steuerelement im AJAX Control Toolkit bietet einen einfachen Popup Effekt: Wenn mit dem Mauszeiger auf ein Element gezeigt wird, wird an einer angegebenen Position ein Popup Fenster angezeigt. Es ist auch möglich, dieses Steuerelement in einem Repeater zu verwenden.

## <a name="steps"></a>Schritte

Zuerst ist eine Datenquelle erforderlich. In diesem Beispiel werden die AdventureWorks-Datenbank und die Microsoft SQL Server 2005 Express Edition verwendet. Die Datenbank ist ein optionaler Teil einer Visual Studio-Installation (einschließlich Express Edition) und ist auch als separater Download unter [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064)verfügbar. Die AdventureWorks-Datenbank ist Teil der SQL Server 2005-Beispiele und-Beispiel Datenbanken (Download bei [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;D isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Die einfachste Möglichkeit, die Datenbank einzurichten, ist die Verwendung des Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;D isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) und das Anfügen der `AdventureWorks.mdf` Datenbankdatei.

Bei diesem Beispiel wird davon ausgegangen, dass die Instanz des SQL Server 2005 Express Edition `SQLEXPRESS` ist und sich auf demselben Computer wie der Webserver befindet. Dies ist auch das Standard Setup. Wenn das Setup abweicht, müssen Sie die Verbindungsinformationen für die Datenbank anpassen.

Um die Funktionalität von ASP.NET AJAX und dem Steuerelement-Toolkit zu aktivieren, muss das `ScriptManager` Steuerelement an einer beliebigen Stelle auf der Seite platziert werden (innerhalb des `<form>` Elements):

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample1.aspx)]

Fügen Sie dann der Seite eine Datenquelle hinzu. Um eine begrenzte Menge von Daten zu verwenden, wählen wir nur die ersten fünf Einträge in der Vendor-Tabelle der AdventureWorks-Datenbank aus. Wenn Sie den Visual Studio-Assistenten verwenden, um die Datenquelle zu erstellen, beachten Sie, dass ein Fehler in der aktuellen Version dem Tabellennamen (`Vendor`) nicht mit `Purchasing`vorangestellt ist. Das folgende Markup zeigt die korrekte Syntax:

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample2.aspx)]

Fügen Sie als nächstes ein Panel hinzu, das als modales Popup fungiert:

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample3.aspx)]

Nun kommt der `HoverMenuExtender` ins Spiel. Damit jedes Element in der Datenquelle ein eigenes Popup erhält, muss der Extender in den `<ItemTemplate>` Abschnitt des Repeater eingefügt werden. Dies ist das Markup:

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample4.aspx)]

Nun zeigt jedes Element in der Datenquelle nach einer Verzögerung von 50 Millisekunden (`PopDelay` Attribut) ein Popup-Element rechts (`PopupPosition`-Attribut) an.

[![das Hover-Menü neben jedem Element im Wiederholungs Modul angezeigt wird.](using-hovermenu-with-a-repeater-control-vb/_static/image2.png)](using-hovermenu-with-a-repeater-control-vb/_static/image1.png)

Das Hover-Menü wird neben jedem Element im Wiederholungs Modul angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-hovermenu-with-a-repeater-control-vb/_static/image3.png)).

> [!div class="step-by-step"]
> [Vorheriges](using-hovermenu-with-a-repeater-control-cs.md)
