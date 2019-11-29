---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-in-a-formview-vb
title: Verwenden von textboxwatermark in einem FormView-Steuerzeichen (VB) | Microsoft-Dokumentation
author: wenz
description: Mit dem textboxwatermark-Steuerelement im AJAX Control Toolkit wird ein Textfeld erweitert, sodass ein Text im Feld angezeigt wird. Wenn ein Benutzer in das Feld klickt,
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 41497361-7fba-4825-b36c-f58d79522a88
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-in-a-formview-vb
msc.type: authoredcontent
ms.openlocfilehash: 1dd17ed57383680122c4ca613ca71f6682dec40e
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74598322"
---
# <a name="using-textboxwatermark-in-a-formview-vb"></a>Verwenden von TextBoxWatermark in einem FormView-Steuerelement (VB)

von [Christian Wenz](https://github.com/wenz)

[Code herunterladen](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark1.vb.zip) oder [PDF herunterladen](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark1VB.pdf)

> Mit dem textboxwatermark-Steuerelement im AJAX Control Toolkit wird ein Textfeld erweitert, sodass ein Text im Feld angezeigt wird. Wenn ein Benutzer auf das Feld klickt, wird es geleert. Wenn der Benutzer das Kontrollkästchen verlässt, ohne Text einzugeben, wird der vorab gefüllte Text erneut angezeigt. Dies ist auch in einem FormView-Steuerelement möglich.

## <a name="overview"></a>Übersicht über

Mit dem `TextBoxWatermark`-Steuerelement im AJAX Control Toolkit wird ein Textfeld erweitert, sodass ein Text im Feld angezeigt wird. Wenn ein Benutzer auf das Feld klickt, wird es geleert. Wenn der Benutzer das Kontrollkästchen verlässt, ohne Text einzugeben, wird der vorab gefüllte Text erneut angezeigt. Dies ist auch in einem `FormView` Steuerelement möglich.

## <a name="steps"></a>Schritte

Zuerst ist eine Datenquelle erforderlich. In diesem Beispiel werden die AdventureWorks-Datenbank und die Microsoft SQL Server 2005 Express Edition verwendet. Die Datenbank ist ein optionaler Teil einer Visual Studio-Installation (einschließlich Express Edition) und ist auch als separater Download unter [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064)verfügbar. Die AdventureWorks-Datenbank ist Teil der SQL Server 2005-Beispiele und-Beispiel Datenbanken (Download bei [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;D isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Die einfachste Möglichkeit, die Datenbank einzurichten, ist die Verwendung des Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;D isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) und das Anfügen der `AdventureWorks.mdf` Datenbankdatei.

Bei diesem Beispiel wird davon ausgegangen, dass die Instanz des SQL Server 2005 Express Edition `SQLEXPRESS` ist und sich auf demselben Computer wie der Webserver befindet. Dies ist auch das Standard Setup. Wenn das Setup abweicht, müssen Sie die Verbindungsinformationen für die Datenbank anpassen.

Um die Funktionalität von ASP.NET AJAX und dem Steuerelement-Toolkit zu aktivieren, muss das `ScriptManager` Steuerelement an einer beliebigen Stelle auf der Seite platziert werden (innerhalb des `<form>` Elements):

[!code-aspx[Main](using-textboxwatermark-in-a-formview-vb/samples/sample1.aspx)]

Fügen Sie dann der Seite eine Datenquelle hinzu, die die SQL-Anweisungen `DELETE`, `INSERT` und `UPDATE` unterstützt. Wenn Sie den Visual Studio-Assistenten verwenden, um die Datenquelle zu erstellen, beachten Sie, dass ein Fehler in der aktuellen Version dem Tabellennamen (`Vendor`) nicht mit `Purchasing`vorangestellt ist. Das folgende Markup zeigt die korrekte Syntax:

[!code-aspx[Main](using-textboxwatermark-in-a-formview-vb/samples/sample2.aspx)]

Merken Sie sich den Namen (`ID`) der Datenquelle, da er in der `DataSourceID`-Eigenschaft des `FormView`-Steuer Elements verwendet wird. Der `<InsertItemTemplate>` Abschnitt des `FormView` enthält ein Textfeld, das durch das `TextBoxWatermarkExtender`-Steuerelement erweitert wird. Stellen Sie sicher, dass sich der Extender in der Vorlage und nicht außerhalb der Vorlage befindet.

[!code-aspx[Main](using-textboxwatermark-in-a-formview-vb/samples/sample3.aspx)]

Wenn sich der Benutzer nun in den Einfügemodus des `FormView` Steuer Elements ändert, wird das Textfeld für den neuen Anbieter durch das `TextBoxWatermarkExtender` Steuerelement vorab ausgefüllt. Wenn Sie in das Textfeld klicken, wird der Fülltext angezeigt.

[![das Wasserzeichen im Feld vom Extender](using-textboxwatermark-in-a-formview-vb/_static/image2.png)](using-textboxwatermark-in-a-formview-vb/_static/image1.png)

Das Wasserzeichen im Feld stammt aus dem Extender ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-textboxwatermark-in-a-formview-vb/_static/image3.png)).

> [!div class="step-by-step"]
> [Zurück](using-textboxwatermark-with-validation-controls-cs.md)
> [Weiter](using-textboxwatermark-with-validation-controls-vb.md)
