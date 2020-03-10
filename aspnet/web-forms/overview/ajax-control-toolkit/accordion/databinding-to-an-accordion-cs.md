---
uid: web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-cs
title: Datenbindung an ein Akkordeon (C#) | Microsoft-Dokumentation
author: wenz
description: Das Steuerelement "Accordion" im AJAX Control Toolkit bietet mehrere Bereiche und ermöglicht es dem Benutzer, jeweils ein Element anzuzeigen. Panels werden normalerweise als w...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 9c8f0054-e319-46f8-80c0-35b606d2fbd4
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-cs
msc.type: authoredcontent
ms.openlocfilehash: 6c28cc958a1de9844627ae16175a5aed153993a8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78498057"
---
# <a name="databinding-to-an-accordion-c"></a>Datenbindung an Accordion (C#)

von [Christian Wenz](https://github.com/wenz)

[Code herunterladen](https://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.cs.zip) oder [PDF herunterladen](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1CS.pdf)

> Das Steuerelement "Accordion" im AJAX Control Toolkit bietet mehrere Bereiche und ermöglicht es dem Benutzer, jeweils ein Element anzuzeigen. Panels werden in der Regel auf der Seite selbst deklariert, aber das Binden an eine Datenquelle bietet mehr Flexibilität.

## <a name="overview"></a>Übersicht

Das Steuerelement "Accordion" im AJAX Control Toolkit bietet mehrere Bereiche und ermöglicht es dem Benutzer, jeweils ein Element anzuzeigen. Panels werden in der Regel auf der Seite selbst deklariert, aber das Binden an eine Datenquelle bietet mehr Flexibilität.

## <a name="steps"></a>Schritte

Zuerst ist eine Datenquelle erforderlich. In diesem Beispiel werden die AdventureWorks-Datenbank und die Microsoft SQL Server 2005 Express Edition verwendet. Die Datenbank ist ein optionaler Teil einer Visual Studio-Installation (einschließlich Express Edition) und ist auch als separater Download unter [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064)verfügbar. Die AdventureWorks-Datenbank ist Teil der SQL Server 2005-Beispiele und-Beispiel Datenbanken (Download bei [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;D isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Die einfachste Möglichkeit, die Datenbank einzurichten, ist die Verwendung des Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;D isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) und das Anfügen der `AdventureWorks.mdf` Datenbankdatei.

Bei diesem Beispiel wird davon ausgegangen, dass die Instanz des SQL Server 2005 Express Edition `SQLEXPRESS` ist und sich auf demselben Computer wie der Webserver befindet. Dies ist auch das Standard Setup. Wenn das Setup abweicht, müssen Sie die Verbindungsinformationen für die Datenbank anpassen.

Um die Funktionalität von ASP.NET AJAX und dem Steuerelement-Toolkit zu aktivieren, muss das `ScriptManager` Steuerelement an einer beliebigen Stelle auf der Seite platziert werden (innerhalb des `<form>` Elements):

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample1.aspx)]

Fügen Sie dann der Seite eine Datenquelle hinzu. Um eine begrenzte Menge von Daten zu verwenden, wählen wir nur die ersten fünf Einträge in der Vendor-Tabelle der AdventureWorks-Datenbank aus. Wenn Sie den Visual Studio-Assistenten verwenden, um die Datenquelle zu erstellen, beachten Sie, dass ein Fehler in der aktuellen Version dem Tabellennamen (`Vendor`) nicht mit `Purchasing`vorangestellt ist. Das folgende Markup zeigt die korrekte Syntax:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample2.aspx)]

Merken Sie sich den Namen (ID) der Datenquelle. Diese sehr Identifikation muss dann in der `DataSourceID`-Eigenschaft des Accordion-Steuer Elements verwendet werden:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample3.aspx)]

Innerhalb des Accordion-Steuer Elements können Sie Vorlagen für verschiedene Teile des Steuer Elements bereitstellen, einschließlich des Headers (`<HeaderTemplate>`) und des Inhalts (`<ContentTemplate>`). Geben Sie in diesen Elementen einfach die Daten aus der Datenquelle mit der `DataBinder.Eval()`-Methode aus:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample4.aspx)]

Wenn die Seite geladen wird, muss die Datenquelle mit diesem serverseitigen Code an das Akkordeon gebunden werden:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample5.aspx)]

Um dieses Beispiel zu schließen, müssen Sie die beiden CSS-Klassen definieren, auf die im Steuerelement "Accordion" verwiesen wird (in den Eigenschaften `HeaderCssClass` und `ContentCssClass`). Fügen Sie das folgende Markup in den `<head>` Abschnitt der Seite ein:

[!code-css[Main](databinding-to-an-accordion-cs/samples/sample6.css)]

[![die Daten im Akkordeon direkt aus der Datenquelle stammen.](databinding-to-an-accordion-cs/_static/image2.png)](databinding-to-an-accordion-cs/_static/image1.png)

Die Daten im Akkordeon stammen direkt aus der Datenquelle ([Klicken Sie, um das Bild in voller Größe anzuzeigen](databinding-to-an-accordion-cs/_static/image3.png)).

> [!div class="step-by-step"]
> [Weiter](dynamically-adding-an-accordion-pane-cs.md)
