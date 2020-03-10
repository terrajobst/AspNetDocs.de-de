---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-vb
title: Verwenden von ModalPopup mit einem Wiederholungs Steuerelement (VB) | Microsoft-Dokumentation
author: wenz
description: Das ModalPopup-Steuerelement im AJAX Control Toolkit bietet eine einfache Möglichkeit zum Erstellen eines modalen Popups mithilfe Client seitiger Mittel. Es ist auch möglich, diese Contr zu verwenden...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 0c8e74f1-b3ba-4ca9-a1c5-f5c4831a359a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 0966770f0218ca91ba7d25e7bf703bf7b005738e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78496839"
---
# <a name="using-modalpopup-with-a-repeater-control-vb"></a>Verwenden von ModalPopup mit einem Wiederholungssteuerelement (VB)

von [Christian Wenz](https://github.com/wenz)

[Code herunterladen](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup2.vb.zip) oder [PDF herunterladen](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup2VB.pdf)

> Das ModalPopup-Steuerelement im AJAX Control Toolkit bietet eine einfache Möglichkeit zum Erstellen eines modalen Popups mithilfe Client seitiger Mittel. Es ist auch möglich, dieses Steuerelement in einem Repeater zu verwenden.

## <a name="overview"></a>Übersicht

Das ModalPopup-Steuerelement im AJAX Control Toolkit bietet eine einfache Möglichkeit zum Erstellen eines modalen Popups mithilfe Client seitiger Mittel. Es ist auch möglich, dieses Steuerelement in einem Repeater zu verwenden.

## <a name="steps"></a>Schritte

Zuerst ist eine Datenquelle erforderlich. In diesem Beispiel werden die AdventureWorks-Datenbank und die Microsoft SQL Server 2005 Express Edition verwendet. Die Datenbank ist ein optionaler Teil einer Visual Studio-Installation (einschließlich Express Edition) und ist auch als separater Download unter [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064)verfügbar. Die AdventureWorks-Datenbank ist Teil der SQL Server 2005-Beispiele und-Beispiel Datenbanken (Download bei [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;D isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Die einfachste Möglichkeit, die Datenbank einzurichten, ist die Verwendung des Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;D isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) und das Anfügen der `AdventureWorks.mdf` Datenbankdatei. Bei diesem Beispiel wird davon ausgegangen, dass die Instanz des SQL Server 2005 Express Edition `SQLEXPRESS` ist und sich auf demselben Computer wie der Webserver befindet. Dies ist auch das Standard Setup. Wenn das Setup abweicht, müssen Sie die Verbindungsinformationen für die Datenbank anpassen. Um die Funktionalität von ASP.NET AJAX und dem Steuerelement-Toolkit zu aktivieren, muss das `ScriptManager` Steuerelement an einer beliebigen Stelle auf der Seite platziert werden (innerhalb des `<form>` Elements):

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-vb/samples/sample1.aspx)]

Fügen Sie dann der Seite eine Datenquelle hinzu. Um eine begrenzte Menge von Daten zu verwenden, wählen wir nur die ersten fünf Einträge in der Vendor-Tabelle der AdventureWorks-Datenbank aus. Wenn Sie den Visual Studio-Assistenten verwenden, um die Datenquelle zu erstellen, beachten Sie, dass ein Fehler in der aktuellen Version dem Tabellennamen (`Vendor`) nicht mit `Purchasing`vorangestellt ist. Das folgende Markup zeigt die korrekte Syntax:

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-vb/samples/sample2.aspx)]

Fügen Sie als nächstes ein Panel hinzu, das als modales Popup fungiert. Sie enthält ein `Button` Steuerelement, um das Popup erneut zu schließen:

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-vb/samples/sample3.aspx)]

Damit das Popup innerhalb des Repeater funktioniert, muss das `ModalPopupExtender` Steuerelement im `<ItemTemplate>` Abschnitt des Wiederholungs Moduls abgelegt werden. Der Bereich befindet sich also außerhalb des Wiederholungs Moduls, aber der Extender befindet sich in. Hier ist das Markup für den Repeater:

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-vb/samples/sample4.aspx)]

Anschließend wird jedes Element in der Datenquelle mit einer Schaltfläche daneben angezeigt, die das modale Popup auslöst.

[![das modale Popup für jeden Datenquellen Eintrag ausgelöst werden kann.](using-modalpopup-with-a-repeater-control-vb/_static/image2.png)](using-modalpopup-with-a-repeater-control-vb/_static/image1.png)

Das modale Popup kann für jeden Datenquellen Eintrag ausgelöst werden ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-modalpopup-with-a-repeater-control-vb/_static/image3.png)).

> [!div class="step-by-step"]
> [Zurück](launching-a-modal-popup-window-from-server-code-vb.md)
> [Weiter](handling-postbacks-from-a-modalpopup-vb.md)
