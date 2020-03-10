---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
title: Hinzufügen einer Spalte zum Modell | Microsoft-Dokumentation
author: shanselman
description: Dies ist ein Einsteiger-Tutorial, in dem die Grundlagen von ASP.NET MVC vorgestellt werden. Erstellen Sie eine einfache Webanwendung, die Daten aus einer Datenbank liest und in diese schreibt.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: 7ae696b9-348f-4993-8ebb-a838acbe0c28
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
msc.type: authoredcontent
ms.openlocfilehash: 1cf092c3db3959d6f47006f1be2ba82833c5dc06
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78437223"
---
# <a name="adding-a-column-to-the-model"></a>Hinzufügen der Spalte zum Modell

von [Scott Hanselman](https://github.com/shanselman)

> Dies ist ein Einsteiger-Tutorial, in dem die Grundlagen von ASP.NET MVC vorgestellt werden. Sie erstellen eine einfache Webanwendung, die Daten aus einer Datenbank liest und in diese schreibt. Besuchen Sie das [ASP.NET MVC Learning Center](../../../index.md) , um weitere ASP.NET MVC-Tutorials und-Beispiele zu finden.

In diesem Abschnitt wird erläutert, wie wir Änderungen am Schema unserer Datenbank vornehmen und die Änderungen in unserer Anwendung behandeln können.

Fügen Sie der Movie-Tabelle eine Spalte "Rating" hinzu. Wechseln Sie zurück zur IDE, und klicken Sie auf den Datenbank-Explorer. Klicken Sie mit der rechten Maustaste auf die Tabelle Movie, und wählen Sie Tabelle öffnen

Fügen Sie eine Spalte "Rating" wie unten gezeigt hinzu. Da wir jetzt über keine Bewertungen verfügen, kann die Spalte NULL-Werte zulassen. Klicken Sie auf Speichern.

[![Bearbeitung der Film Tabelle](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)

Kehren Sie als nächstes zum Projektmappen-Explorer zurück, und öffnen Sie die Datei "Movies. edmx" (die sich im Ordner "\models" befindet). Klicken Sie mit der rechten Maustaste auf die Entwurfs Oberfläche (der weiße Bereich), und wählen Sie Modell aus Datenbank aktualisieren aus.

[![Filme-Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)

Dadurch wird der Update-Assistent gestartet. Klicken Sie darin auf die Registerkarte Aktualisieren und dann auf Fertigstellen. Unsere Movie-Modell Klasse wird dann mit der neuen Spalte aktualisiert.

![Update-Assistent (2)](getting-started-with-mvc-part8/_static/image5.png)

Nachdem Sie auf Fertigstellen geklickt haben, können Sie sehen, dass die neue Bewertungs Spalte der Movie-Entität in unserem Modell hinzugefügt wurde.

[![Movie-Entität](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)

Wir haben eine Spalte im Datenbankmodell hinzugefügt, aber die Ansichten wissen es nicht.

## <a name="update-views-with-model-changes"></a>Aktualisieren von Sichten mit Modelländerungen

Es gibt mehrere Möglichkeiten, unsere Ansichts Vorlagen zu aktualisieren, um die neue Bewertungs Spalte widerzuspiegeln. Da wir diese Ansichten erstellt haben, indem Sie Sie über das Dialogfeld "Ansicht hinzufügen" erstellt haben, können wir Sie löschen und erneut erstellen. Allerdings haben Benutzer in der Regel bereits Änderungen an Ihren Ansichts Vorlagen von der anfänglichen Gerüst Generierung vorgenommen, und Sie möchten Felder manuell hinzufügen oder löschen, genauso wie das ID-Feld für Create.

Öffnen Sie die Vorlage "\views\movies\index.aspx", und fügen Sie dem Anfang der Film Tabelle eine &lt;Th-&gt;Bewertung&lt;/Th-&gt; hinzu. Ich habe meine nach Genre hinzugefügt. Fügen Sie dann in derselben Spaltenposition, aber unten unten eine Zeile hinzu, um unsere neue Bewertung auszugeben.

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample1.aspx)]

Unsere endgültige Vorlage "index. aspx" sieht wie folgt aus:

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample2.aspx)]

Öffnen Sie dann die Vorlage "\views\movies\kreate.aspx", und fügen Sie eine Bezeichnung und ein Textfeld für die neue Bewertungs Eigenschaft hinzu:

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample3.aspx)]

Unsere endgültige Vorlage "Create. aspx" sieht wie folgt aus, und wir ändern den Titel des Browsers und den sekundären &lt;H2&gt; Titel in etwas wie "Erstellen eines Films", während wir hier sind!

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample4.aspx)]

Führen Sie Ihre APP aus, und jetzt haben Sie ein neues Feld in der Datenbank, das der Seite erstellen hinzugefügt wurde. Fügen Sie einen neuen Film hinzu: dieses Mal mit einer Bewertung, und klicken Sie auf erstellen.

[![Erstellen eines Films: Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)

Nachdem Sie auf Erstellen klicken, werden Sie an die Index Seite gesendet, auf der Sie einen neuen Film mit der neuen Bewertungs Spalte in der Datenbank auflisten.

[![Movie List-Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)

In diesem grundlegenden Tutorial haben Sie mit dem Erstellen von Controllern begonnen, indem Sie Ihnen Ansichten zuordnen und hart codierte Daten übergeben. Anschließend haben wir eine Datenbank erstellt und entworfen und einige Daten in Sie eingefügt. Wir haben die Daten aus der Datenbank abgerufen und in einer HTML-Tabelle angezeigt. Anschließend haben wir ein Erstellungs Formular hinzugefügt, mit dem der Benutzer der Datenbank selbst innerhalb der Webanwendung Daten hinzufügen können. Wir haben die Validierung hinzugefügt und dann die Validierung für JavaScript auf Clientseite verwendet. Schließlich haben wir die Datenbank so geändert, dass Sie eine neue Datenspalte enthält, und dann unsere beiden Seiten aktualisiert, um diese neuen Daten zu erstellen und anzuzeigen.

Ich empfehle Ihnen jetzt, das Tutorial "[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)" in der Zwischenebene und die vielen Videos und Ressourcen in [https://asp.net/mvc](https://asp.net/mvc) kennenzulernen, um noch mehr über ASP.NET MVC zu erfahren.

Viel Spaß!

- Scott Hanselman- [http://hanselman.com](http://hanselman.com) und [@shanselman](http://twitter.com/shanselman) auf Twitter.

> [!div class="step-by-step"]
> [Previous](getting-started-with-mvc-part7.md)
