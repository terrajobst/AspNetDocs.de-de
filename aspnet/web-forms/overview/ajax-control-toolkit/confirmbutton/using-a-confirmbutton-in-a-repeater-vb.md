---
uid: web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-vb
title: Verwenden von ConfirmButton in einem Repeater (VB) | Microsoft-Dokumentation
author: wenz
description: Der ConfirmButton-Extender im AJAX Control Toolkit erstellt das Popup "Yes/No", wenn der Benutzer auf eine Schaltfläche klickt (einschließlich des LinkButton-Steuer Elements). Nur wenn ja...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 18c31709-3f9d-4d93-8b01-f1356bf610b4
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-vb
msc.type: authoredcontent
ms.openlocfilehash: 001233d866d8a731d93d6900f714cd2060f3d08c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78497541"
---
# <a name="using-a-confirmbutton-in-a-repeater-vb"></a>Verwenden eines ConfirmButton-Steuerelements in einem Wiederholungssteuerelement (VB)

von [Christian Wenz](https://github.com/wenz)

[Code herunterladen](https://download.microsoft.com/download/8/6/d/86dea6c6-bb92-4fa6-aa14-f8c0f82100f5/ConfirmButton1.vb.zip) oder [PDF herunterladen](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/confirmbutton1VB.pdf)

> Der ConfirmButton-Extender im AJAX Control Toolkit erstellt das Popup "Yes/No", wenn der Benutzer auf eine Schaltfläche klickt (einschließlich des LinkButton-Steuer Elements). Nur wenn auf Ja geklickt wird, wird die Aktion der Schaltfläche ausgeführt und andernfalls abgebrochen. Dies ist auch in einem Repeater möglich.

## <a name="overview"></a>Übersicht

Der ConfirmButton-Extender im AJAX Control Toolkit erstellt das Popup "Yes/No", wenn der Benutzer auf eine Schaltfläche klickt (einschließlich des LinkButton-Steuer Elements). Nur wenn auf Ja geklickt wird, wird die Aktion der Schaltfläche ausgeführt und andernfalls abgebrochen. Dies ist auch in einem Repeater möglich.

## <a name="steps"></a>Schritte

Zuerst ist eine Datenquelle erforderlich. In diesem Beispiel werden die AdventureWorks-Datenbank und die Microsoft SQL Server 2005 Express Edition verwendet. Die Datenbank ist ein optionaler Teil einer Visual Studio-Installation (einschließlich Express Edition) und ist auch als separater Download unter [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064)verfügbar. Die AdventureWorks-Datenbank ist Teil der SQL Server 2005-Beispiele und-Beispiel Datenbanken (Download bei [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;D isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Die einfachste Möglichkeit, die Datenbank einzurichten, ist die Verwendung des Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;D isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) und das Anfügen der `AdventureWorks.mdf` Datenbankdatei.

Bei diesem Beispiel wird davon ausgegangen, dass die Instanz des SQL Server 2005 Express Edition `SQLEXPRESS` ist und sich auf demselben Computer wie der Webserver befindet. Dies ist auch das Standard Setup. Wenn das Setup abweicht, müssen Sie die Verbindungsinformationen für die Datenbank anpassen.

Um die Funktionalität von ASP.NET AJAX und dem Steuerelement-Toolkit zu aktivieren, muss das `ScriptManager` Steuerelement an einer beliebigen Stelle auf der Seite platziert werden (innerhalb des `<form>` Elements):

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample1.aspx)]

Anschließend ist eine Datenquelle erforderlich. Der Einfachheit halber werden nur die ersten fünf Einträge in der Tabelle "Lieferanten" von AdventureWorks abgerufen. Beachten Sie, dass bei der Verwendung des Visual Studio-Assistenten zum Erstellen der Datenquelle der Tabellenname (`Vendors`) derzeit nicht ordnungsgemäß mit dem Präfix `Purchasing`versehen ist. Das folgende Markup ist das richtige Markup:

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample2.aspx)]

Diese Datenquelle kann dann in einem Repeater verwendet werden. Wie üblich Ruft die `DataBinder.Eval()`-Methode Daten aus der Datenquelle ab. Das `ConfirmButtonExtender` Steuerelement muss dann im `<ItemTemplate>` Abschnitt des Wiederholungs Moduls platziert werden, damit es für jeden Eintrag in der Datenquelle angezeigt wird.

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample3.aspx)]

[![die Schaltfläche bestätigen neben jedem Eintrag aus der Datenquelle angezeigt wird.](using-a-confirmbutton-in-a-repeater-vb/_static/image2.png)](using-a-confirmbutton-in-a-repeater-vb/_static/image1.png)

Die Schaltfläche bestätigen wird neben jedem Eintrag aus der Datenquelle angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-a-confirmbutton-in-a-repeater-vb/_static/image3.png)).

> [!div class="step-by-step"]
> [Previous](using-a-confirmbutton-in-a-repeater-cs.md)
