---
uid: mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-cs
title: Erstellen von Seiten Layouts mit Ansichts MasterC#Seiten () | Microsoft-Dokumentation
author: microsoft
description: In diesem Tutorial erfahren Sie, wie Sie ein gängiges Seitenlayout für mehrere Seiten in Ihrer Anwendung erstellen, indem Sie die Masterseiten der Ansicht nutzen. Sie können eine...
ms.author: riande
ms.date: 10/16/2008
ms.assetid: dff54fcb-68b1-4488-89a2-ca97532d6a4c
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 026e3efb4ebf84016aa0f6a5fda4af549fdadfcb
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74594184"
---
# <a name="creating-page-layouts-with-view-master-pages-c"></a>Erstellen von Seitenlayouts mit Ansichtsmasterseiten (C#)

von [Microsoft](https://github.com/microsoft)

[PDF herunterladen](https://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_12_CS.pdf)

> In diesem Tutorial erfahren Sie, wie Sie ein gängiges Seitenlayout für mehrere Seiten in Ihrer Anwendung erstellen, indem Sie die Masterseiten der Ansicht nutzen. Sie können z. b. eine Ansichts Master Seite verwenden, um ein zweispaltige Seitenlayout zu definieren und das zweispaltige Layout für alle Seiten in der Webanwendung zu verwenden.

## <a name="creating-page-layouts-with-view-master-pages"></a>Erstellen von Seiten Layouts mit Ansichts Master Seiten

In diesem Tutorial erfahren Sie, wie Sie ein gängiges Seitenlayout für mehrere Seiten in Ihrer Anwendung erstellen, indem Sie die Masterseiten der Ansicht nutzen. Sie können z. b. eine Ansichts Master Seite verwenden, um ein zweispaltige Seitenlayout zu definieren und das zweispaltige Layout für alle Seiten in der Webanwendung zu verwenden.

Sie können auch die Masterseiten der Ansicht nutzen, um gemeinsame Inhalte für mehrere Seiten in Ihrer Anwendung freizugeben. Beispielsweise können Sie das Website Logo, Navigationslinks und Banner Ankündigungen in eine Ansichts Master Seite platzieren. Auf diese Weise wird dieser Inhalt von jeder Seite in der Anwendung automatisch angezeigt.

In diesem Tutorial erfahren Sie, wie Sie eine neue Ansichts Master Seite erstellen und eine neue Seite Inhaltsseite auf der Grundlage der Master Seite erstellen.

### <a name="creating-a-view-master-page"></a>Erstellen einer Ansichts Master Seite

Erstellen Sie zunächst eine Ansichts Master Seite, die ein zweispaltige Layout definiert. Sie fügen eine neue Ansichts Master Seite zu einem MVC-Projekt hinzu, indem Sie mit der rechten Maustaste auf den Ordner views\shared klicken, die Menüoption **hinzufügen, neues Element**und die Vorlage **MVC-Ansichts Master Seite** auswählen (siehe Abbildung 1).

[![Hinzufügen einer Ansichts Master Seite](creating-page-layouts-with-view-master-pages-cs/_static/image2.png)](creating-page-layouts-with-view-master-pages-cs/_static/image1.png)

**Abbildung 01**: Hinzufügen einer Ansichts Master Seite ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-page-layouts-with-view-master-pages-cs/_static/image3.png))

Sie können in einer Anwendung mehr als eine Ansichts Master Seite erstellen. Jede Ansichts Master Seite kann ein anderes Seitenlayout definieren. Beispielsweise möchten Sie möglicherweise, dass bestimmte Seiten über ein Layout mit zwei Spalten verfügen und andere Seiten ein Layout mit drei Spalten aufweisen.

Eine Ansichts Master Seite sieht in etwa wie eine standardmäßige ASP.NET-MVC-Ansicht aus. Anders als bei einer normalen Ansicht enthält eine Ansichts Master Seite jedoch mindestens einen `<asp:ContentPlaceHolder>` Tags. Die `<contentplaceholder>` Tags werden verwendet, um die Bereiche der Master Seite zu markieren, die auf einer einzelnen Inhaltsseite überschrieben werden können.

Beispielsweise wird auf der Seite Master anzeigen in der Liste 1 ein zweispaltige Layout definiert. Sie enthält zwei `<contentplaceholder>` Tags. Eine `<ContentPlaceHolder>` für jede Spalte.

**Codebeispiel 1 – `Views\Shared\Site.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample1.aspx)]

Der Text der Ansichts Master Seite in der Liste 1 enthält zwei `<div>` Tags, die den beiden Spalten entsprechen. Die Cascading Stylesheet-Spalten Klasse wird auf beide `<div>` Tags angewendet. Diese Klasse wird in dem Stylesheet definiert, das am oberen Rand der Master Seite deklariert wird. Sie können eine Vorschau anzeigen, wie die Ansichts Master Seite gerendert wird, indem Sie zu Designansicht wechseln. Klicken Sie unten links im Quellcode-Editor auf die Registerkarte Entwurf (siehe Abbildung 2).

[![Anzeigen der Vorschau einer Master Seite im Designer](creating-page-layouts-with-view-master-pages-cs/_static/image5.png)](creating-page-layouts-with-view-master-pages-cs/_static/image4.png)

**Abbildung 02**: Anzeigen einer Vorschau einer Master Seite im Designer ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-page-layouts-with-view-master-pages-cs/_static/image6.png))

### <a name="creating-a-view-content-page"></a>Erstellen einer Seite "Inhalt anzeigen"

Nachdem Sie eine Ansichts Master Seite erstellt haben, können Sie eine oder mehrere Inhaltsseiten anzeigen, basierend auf der Seite Master anzeigen. Sie können z. b. eine Inhaltsseite für die Index Ansicht für den Home-Controller erstellen, indem Sie mit der rechten Maustaste auf den Ordner views\home klicken, **hinzufügen, neues Element**auswählen, die Vorlage für die **Inhaltsseite für die MVC-Ansicht** eingeben, den Namen index. aspx eingeben und auf die Schaltfläche **Hinzufügen** klicken (siehe Abbildung 3

[![Hinzufügen einer Ansichts Inhaltsseite](creating-page-layouts-with-view-master-pages-cs/_static/image8.png)](creating-page-layouts-with-view-master-pages-cs/_static/image7.png)

**Abbildung 03**: Hinzufügen einer Ansichts Inhaltsseite ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-page-layouts-with-view-master-pages-cs/_static/image9.png))

Nachdem Sie auf die Schaltfläche Hinzufügen geklickt haben, wird ein neues Dialogfeld angezeigt, in dem Sie eine Ansichts Master Seite auswählen können, die mit der Seite Inhalt anzeigen verknüpft werden soll (siehe Abbildung 4). Sie können zur Master Seite "Site. Master View" navigieren, die wir im vorherigen Abschnitt erstellt haben.

[![Auswählen einer Master Seite](creating-page-layouts-with-view-master-pages-cs/_static/image11.png)](creating-page-layouts-with-view-master-pages-cs/_static/image10.png)

**Abbildung 04**: Auswählen einer Master Seite ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-page-layouts-with-view-master-pages-cs/_static/image12.png))

Nachdem Sie basierend auf der Seite "Site. Master Master" eine neue Seite Inhaltsseite erstellt haben, wird die Datei in der Liste 2 angezeigt.

**Codebeispiel 2 – `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample2.aspx)]

Beachten Sie, dass diese Ansicht ein `<asp:Content>`-Tag enthält, das den einzelnen `<asp:ContentPlaceHolder>`-Tags auf der Seite Master anzeigen entspricht. Jedes `<asp:Content>` Tag enthält ein ContentPlaceHolderID-Attribut, das auf die bestimmte `<asp:ContentPlaceHolder>` verweist, die es überschreibt.

Beachten Sie außerdem, dass die Seite Inhaltsansicht in der Liste 2 keine normalen öffnenden und schließenden HTML-Tags enthält. Er enthält z. b. nicht die öffnenden und schließenden `<html>` oder `<head>` Tags. Alle normalen öffnenden und schließenden Tags sind auf der Ansichts Master Seite enthalten.

Alle Inhalte, die Sie auf der Seite Inhalt anzeigen anzeigen möchten, müssen in einem `<asp:Content>`-Tag abgelegt werden. Wenn Sie HTML-Inhalte oder andere Inhalte außerhalb dieser Tags platzieren, erhalten Sie eine Fehlermeldung, wenn Sie versuchen, die Seite anzuzeigen.

Sie müssen nicht jedes `<asp:ContentPlaceHolder>`-Tag von einer Master Seite auf einer Inhalts Ansichts Seite überschreiben. Sie müssen nur ein `<asp:ContentPlaceHolder>`-Tag überschreiben, wenn Sie das-Tag durch bestimmten Inhalt ersetzen möchten.

Beispielsweise enthält die geänderte Index Sicht in der Liste 3 nur zwei `<asp:Content>` Tags. Alle `<asp:Content>` Tags enthalten Text.

**Codebeispiel 3 – `Views\Home\Index.aspx (modified)`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample3.aspx)]

Wenn die Ansicht in der Liste 3 angefordert wird, wird die Seite in Abbildung 5 gerendert. Beachten Sie, dass die Sicht eine Seite mit zwei Spalten rendert. Beachten Sie außerdem, dass der Inhalt der Seite Inhalt anzeigen mit dem Inhalt der Seite Master anzeigen zusammengeführt wird.

[![der Seite "Inhalt der Index Ansicht"](creating-page-layouts-with-view-master-pages-cs/_static/image14.png)](creating-page-layouts-with-view-master-pages-cs/_static/image13.png)

**Abbildung 05**: die Inhaltsseite der Index Ansicht ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-page-layouts-with-view-master-pages-cs/_static/image15.png))

### <a name="modifying-view-master-page-content"></a>Ändern des Inhalts der Sicht Master Seite

Ein Problem, das bei der Arbeit mit dem Anzeigen von Masterseiten fast sofort auftritt, ist das Problem beim Ändern von Ansichts-Masterseiten Inhalten, wenn verschiedene Ansichts Inhaltsseiten angefordert werden. Beispielsweise möchten Sie, dass jede Seite in Ihrer Webanwendung einen eindeutigen Titel hat. Der Titel wird jedoch auf der Seite Master anzeigen und nicht auf der Seite Inhalt anzeigen deklariert. Wie passen Sie also den Seitentitel für jede Inhaltsseite der Ansicht an?

Es gibt zwei Möglichkeiten, wie Sie den Titel ändern können, der auf der Seite Inhalt anzeigen angezeigt wird. Zuerst können Sie dem Title-Attribut der `<%@ page %>` Direktive, die oben auf der Seite Inhalt anzeigen deklariert wurde, einen Seitentitel zuweisen. Wenn Sie z. b. der Index Ansicht den Seitentitel "super tolle Website" zuweisen möchten, können Sie die folgende Direktive am Anfang der Index Ansicht einschließen:

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample4.aspx)]

Wenn die Index Ansicht im Browser gerendert wird, wird der gewünschte Titel in der Titelleiste des Browsers angezeigt:

[Titelleiste des ![Browsers](creating-page-layouts-with-view-master-pages-cs/_static/image17.png)](creating-page-layouts-with-view-master-pages-cs/_static/image16.png)

Eine Master Ansichts Seite muss eine wichtige Voraussetzung erfüllen, damit das Title-Attribut funktioniert. Die Ansichts Master Seite muss eine `<head runat="server">` Kennung anstelle eines normalen `<head>`-Tags für den zugehörigen Header enthalten. Wenn das `<head>`-Tag das runat = "Server"-Attribut nicht enthält, wird der Titel nicht angezeigt. Die standardmäßige Ansichts Master Seite enthält die erforderliche `<head runat="server">` Kennung.

Eine alternative Methode zum Ändern des Inhalts von Masterseiten von einer einzelnen Ansichts Inhaltsseite besteht darin, den Bereich, den Sie ändern möchten, in einem `<asp:ContentPlaceHolder>`-Tag zu schließen. Stellen Sie sich beispielsweise vor, dass Sie nicht nur den Titel, sondern auch die Meta-Tags ändern möchten, die von einer Master Ansichts Seite gerendert werden. Die Seite Master Ansicht in der Liste 4 enthält ein `<asp:ContentPlaceHolder>`-Tag innerhalb des `<head>`-Tags.

**Codebeispiel 4 – `Views\Shared\Site2.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample5.aspx)]

Beachten Sie, dass das `<asp:ContentPlaceHolder>`-Tag in der Liste 4 Standard Inhalt enthält: einen Standard Titel und standardmeta-Tags. Wenn Sie dieses `<asp:ContentPlaceHolder>`-Tag nicht auf einer einzelnen Inhaltsseite anzeigen außer Kraft setzen, wird der Standard Inhalt angezeigt.

Die Seite Inhaltsansicht in der Liste 5 überschreibt das `<asp:ContentPlaceHolder>`-Tag, um einen benutzerdefinierten Titel und benutzerdefinierte Meta-Tags anzuzeigen.

**Codebeispiel 5 – `Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample6.aspx)]

### <a name="summary"></a>Summary

In diesem Tutorial haben Sie eine grundlegende Einführung zum Anzeigen von Masterseiten und zum Anzeigen von Inhaltsseiten bereitgestellt. Sie haben gelernt, wie Sie neue Ansichts Masterseiten erstellen und Inhaltsseiten auf der Grundlage dieser Seiten erstellen können. Wir haben auch erläutert, wie Sie den Inhalt einer Ansichts Master Seite auf einer bestimmten Seite Inhaltsseite ändern können.

> [!div class="step-by-step"]
> [Zurück](using-the-tagbuilder-class-to-build-html-helpers-cs.md)
> [Weiter](passing-data-to-view-master-pages-cs.md)
