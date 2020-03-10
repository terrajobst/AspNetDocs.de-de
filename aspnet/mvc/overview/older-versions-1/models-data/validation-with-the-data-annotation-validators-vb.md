---
uid: mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-vb
title: Validierung mit den Validierungs Steuerelementen für Daten Anmerkungen (VB) | Microsoft-Dokumentation
author: microsoft
description: Nutzen Sie den Modell Binder für die Daten Anmerkung, um die Validierung innerhalb einer ASP.NET-MVC-Anwendung auszuführen. Erfahren Sie, wie Sie die verschiedenen Typen von Validator verwenden...
ms.author: riande
ms.date: 05/29/2009
ms.assetid: 0d23ff2b-f2ec-434a-be3b-1180beeccba3
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-vb
msc.type: authoredcontent
ms.openlocfilehash: 00150575baabc659f7dd0c07349cde52105f6c8b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78435939"
---
# <a name="validation-with-the-data-annotation-validators-vb"></a>Überprüfung der Validierungssteuerelemente für Datenanmerkungen (VB)

von [Microsoft](https://github.com/microsoft)

> Nutzen Sie den Modell Binder für die Daten Anmerkung, um die Validierung innerhalb einer ASP.NET-MVC-Anwendung auszuführen. Erfahren Sie, wie Sie die verschiedenen Typen von Prüfungs-Attributen verwenden und in der Microsoft-Entity Framework arbeiten.

In diesem Tutorial erfahren Sie, wie Sie mit den Validierungs Steuerelementen für Daten Anmerkungen eine Validierung in einer ASP.NET MVC-Anwendung ausführen. Der Vorteil der Verwendung von Validierungs Steuerelementen für Daten Anmerkungen besteht darin, dass Sie eine Validierung durchführen können, indem Sie ein oder mehrere Attribute – z. b. das required-Attribut oder das StringLength-Attribut – einer Klassen Eigenschaft hinzufügen.

Bevor Sie die Validierungs Steuerelemente für Daten Anmerkungen verwenden können, müssen Sie den Daten Annotations-Modell Binder herunterladen. Sie können das Beispielmodell Binder für Daten Anmerkungen von der CodePlex-Website herunterladen, indem Sie [hier](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471)klicken.

Es ist wichtig zu verstehen, dass der Modell Binder für Daten Anmerkungen kein offizieller Teil des Microsoft ASP.NET MVC-Frameworks ist. Obwohl der Daten Annotations-Modell Binder vom Microsoft ASP.NET MVC-Team erstellt wurde, bietet Microsoft keinen offiziellen Produktsupport für den in diesem Tutorial beschriebenen Daten Annotations-Modell Binder.

## <a name="using-the-data-annotation-model-binder"></a>Verwenden des Daten Annotation-Modell Binders

Um den Modell Binder für Daten Anmerkungen in einer ASP.NET MVC-Anwendung zu verwenden, müssen Sie zunächst einen Verweis auf die Microsoft. Web. MVC. DataAnnotations. dll-Assembly und die System. ComponentModel. DataAnnotations. dll-Assembly hinzufügen. Wählen Sie die Menüoption **Projekt, Verweis hinzufügen**aus. Klicken Sie anschließend auf die Registerkarte **Durchsuchen** , und navigieren Sie zu dem Verzeichnis, in das Sie das Beispiel für das Daten Annotations-Modell Binder heruntergeladen (und entzippt **) haben**.

[![](validation-with-the-data-annotation-validators-vb/_static/image2.png)](validation-with-the-data-annotation-validators-vb/_static/image1.png)

**Abbildung 1**: Hinzufügen eines Verweises auf den Modell Binder für Daten Anmerkungen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](validation-with-the-data-annotation-validators-vb/_static/image3.png))

Wählen Sie sowohl die Microsoft. Web. MVC. DataAnnotations. dll-Assembly als auch die System. ComponentModel. DataAnnotations. dll-Assembly aus, und klicken Sie auf die Schaltfläche **OK** .

Die in .NET Framework Service Pack 1 enthaltene Assembly System. ComponentModel. DataAnnotations. dll kann nicht mit dem Modell Binder für Daten Anmerkungen verwendet werden. Sie müssen die Version der System. ComponentModel. DataAnnotations. dll-Assembly verwenden, die im Beispiel für den Daten Annotations-Modell Binder Download enthalten ist.

Schließlich müssen Sie den DataAnnotations-Modell Binder in der Datei Global. asax registrieren. Fügen Sie die folgende Codezeile der Anwendung\_Start ()-Ereignishandler hinzu, damit die Anwendung\_Start ()-Methode wie folgt aussieht:

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample1.vb)]

Diese Codezeile registriert dataannotationsmodelbinder als Standardmodell Binder für die gesamte ASP.NET MVC-Anwendung.

## <a name="using-the-data-annotation-validator-attributes"></a>Verwenden der Attribute für Validierungs Steuerelemente für Daten Anmerkungen

Wenn Sie den Modell Binder für Daten Anmerkungen verwenden, verwenden Sie Validierungs Steuerelement Attribute, um die Validierung auszuführen. Der System. ComponentModel. DataAnnotations-Namespace enthält die folgenden Validierungs Steuerelement Attribute:

- Range – Hiermit können Sie überprüfen, ob der Wert einer Eigenschaft zwischen einem angegebenen Wertebereich liegt.
- RegularExpression – Hiermit können Sie überprüfen, ob der Wert einer Eigenschaft mit einem angegebenen Muster für reguläre Ausdrücke übereinstimmt.
- Erforderlich – Hiermit können Sie eine Eigenschaft als erforderlich kennzeichnen.
- StringLength – ermöglicht es Ihnen, eine maximale Länge für eine Zeichen folgen Eigenschaft anzugeben.
- Validierung – die Basisklasse für alle Prüfungs-Attribute.

> [!NOTE] 
> 
> Wenn die Überprüfungsanforderungen von keinem der Standard Validierungs Steuerelemente erfüllt werden, haben Sie immer die Möglichkeit, ein benutzerdefiniertes Validierungs Steuerelement-Attribut zu erstellen, indem Sie ein neues Prüfungs-Attribut vom Basis Validierungs Attribut erben.

Die Product-Klasse in der **Liste 1** veranschaulicht, wie diese Prüfungs-Attribute verwendet werden. Die Eigenschaften "Name", "Beschreibung" und "UnitPrice" werden als erforderlich markiert. Die Name-Eigenschaft muss eine Zeichen folgen Länge von weniger als 10 Zeichen aufweisen. Zum Schluss muss die UnitPrice-Eigenschaft mit einem Muster für reguläre Ausdrücke, das einen Währungsbetrag darstellt, identisch sein.

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample2.vb)]

Codebeispiel **1**: models\product.vb

Die Product-Klasse veranschaulicht, wie ein zusätzliches Attribut verwendet wird: das Display Name-Attribut. Mit dem Display Name-Attribut können Sie den Namen der Eigenschaft ändern, wenn die Eigenschaft in einer Fehlermeldung angezeigt wird. Anstatt die Fehlermeldung "das UnitPrice-Feld ist erforderlich" anzuzeigen, können Sie die Fehlermeldung "das Preis Feld ist erforderlich" anzeigen.

> [!NOTE] 
> 
> Wenn Sie die von einem Validierungs Steuerelement angezeigte Fehlermeldung vollständig anpassen möchten, können Sie der ErrorMessage-Eigenschaft des Validierungs Steuer Elements wie folgt eine benutzerdefinierte Fehlermeldung zuweisen: `<Required(ErrorMessage:="This field needs a value!")>`

Sie können die Product-Klasse in der **Liste 1** mit der Aktion "Create () Controller" in der **Liste 2**verwenden. Diese Controller Aktion zeigt die Ansicht erstellen erneut an, wenn der Modell Zustand Fehler enthält.

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample3.vb)]

Codebeispiel **2**: controllers\productcontroller.vb

Zum Schluss können Sie die Ansicht in der **Liste 3** erstellen, indem Sie mit der rechten Maustaste auf die Aktion erstellen () klicken und die Menüoption **Ansicht hinzufügen**auswählen. Erstellen Sie eine stark typisierte Ansicht mit der Product-Klasse als Modell Klasse. Wählen Sie in der Dropdown Liste Inhalt anzeigen die Option **Erstellen** aus (siehe **Abbildung 2**).

[![](validation-with-the-data-annotation-validators-vb/_static/image5.png)](validation-with-the-data-annotation-validators-vb/_static/image4.png)

**Abbildung 2**: Hinzufügen der CREATE VIEW

[!code-aspx[Main](validation-with-the-data-annotation-validators-vb/samples/sample4.aspx)]

Codebeispiel **3**: views\product\kreate.aspx

> [!NOTE] 
> 
> Entfernen Sie das Feld ID aus dem Formular erstellen, das von der Menüoption **Ansicht hinzufügen** generiert wurde. Da das ID-Feld einer Identitäts Spalte entspricht, sollten Sie nicht zulassen, dass Benutzer einen Wert für dieses Feld eingeben.

Wenn Sie das Formular zum Erstellen eines Produkts übermitteln und keine Werte für die erforderlichen Felder eingeben, werden die Validierungs Fehlermeldungen in **Abbildung 3** angezeigt.

[![](validation-with-the-data-annotation-validators-vb/_static/image7.png)](validation-with-the-data-annotation-validators-vb/_static/image6.png)

**Abbildung 3**: Fehlende erforderliche Felder

Wenn Sie einen ungültigen Währungsbetrag eingeben, wird die Fehlermeldung in **Abbildung 4** angezeigt.

[![](validation-with-the-data-annotation-validators-vb/_static/image9.png)](validation-with-the-data-annotation-validators-vb/_static/image8.png)

**Abbildung 4**: Ungültige Währungs Menge

## <a name="using-data-annotation-validators-with-the-entity-framework"></a>Verwenden von Validierungs Steuerelementen für Daten Anmerkungen mit dem Entity Framework

Wenn Sie die Microsoft-Entity Framework verwenden, um die Datenmodell Klassen zu generieren, können Sie die Validierungs Steuerelement-Attribute nicht direkt auf die Klassen anwenden. Da der Entity Framework Designer die Modellklassen generiert, werden alle Änderungen, die Sie an den Modellklassen vornehmen, bei der nächsten Änderung im Designer überschrieben.

Wenn Sie die Validierungs Steuerelemente mit den vom Entity Framework generierten Klassen verwenden möchten, müssen Sie Meta-Daten Klassen erstellen. Sie wenden die Validierungs Steuerelemente auf die Meta Data-Klasse an, anstatt die Validierungs Steuerelemente auf die tatsächliche Klasse anzuwenden.

Angenommen, Sie haben eine Movie-Klasse mit der Entity Framework erstellt (siehe **Abbildung 5**). Stellen Sie sich außerdem vor, dass Sie die Eigenschaften "Movie Title" und "Director" Eigenschaften benötigen. In diesem Fall können Sie die partielle Klasse und die Meta Data-Klasse in der **Liste 4**erstellen.

[![](validation-with-the-data-annotation-validators-vb/_static/image11.png)](validation-with-the-data-annotation-validators-vb/_static/image10.png)

**Abbildung 5**: von Entity Framework generierte Movie-Klasse

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample5.vb)]

Codebeispiel **4**: models\muvie.vb

Die Datei in der **Liste 4** enthält zwei Klassen mit den Namen "Movie" und "muviemetadata". Die Movie-Klasse ist eine partielle Klasse. Sie entspricht der partiellen Klasse, die von dem Entity Framework generiert wird, der in der Datei "Datamodel. Designer. vb" enthalten ist.

Derzeit unterstützt .NET Framework keine partiellen Eigenschaften. Daher gibt es keine Möglichkeit, die Prüfungs-Attribute auf die Eigenschaften der Movie-Klasse anzuwenden, die in der Datei "Datamodel. Designer. vb" definiert ist, indem die Prüfungs Attribute auf die Eigenschaften der Filmklasse angewendet werden, die in der Datei in der **Liste 4**definiert ist.

Beachten Sie, dass die partielle Movie-Klasse mit einem metadataType-Attribut ergänzt wird, das auf die Klasse "Klasse" verweist. Die Klasse "mviemetadata" enthält Proxy Eigenschaften für die Eigenschaften der Movie-Klasse.

Die Validierungs Steuerelement-Attribute werden auf die Eigenschaften der Klasse "" der Klasse "Klasse" angewendet. Die Eigenschaften Title, Director und datereleasing werden alle als erforderliche Eigenschaften gekennzeichnet. Der Director-Eigenschaft muss eine Zeichenfolge mit weniger als 5 Zeichen zugewiesen werden. Schließlich wird das Display Name-Attribut auf die datereleasing-Eigenschaft angewendet, um eine Fehlermeldung wie "das freigegebene Feld" Date "ist erforderlich anzuzeigen. anstelle des Fehlers "das datereleasing-Feld ist erforderlich".

> [!NOTE] 
> 
> Beachten Sie, dass die Proxy Eigenschaften in der Klasse "porviemetadata" nicht die gleichen Typen wie die entsprechenden Eigenschaften in der Klasse "Movie" darstellen müssen. Beispielsweise ist die Director-Eigenschaft eine Zeichen folgen Eigenschaft in der Movie-Klasse und eine Object-Eigenschaft in der Klasse "Klasse".

Die Seite in **Abbildung 6** zeigt die Fehlermeldungen, die zurückgegeben werden, wenn Sie ungültige Werte für die Filmeigenschaften eingeben.

[![](validation-with-the-data-annotation-validators-vb/_static/image13.png)](validation-with-the-data-annotation-validators-vb/_static/image12.png)

**Abbildung 6**: Verwenden von Validierungs Steuerelemente mit dem Entity Framework ([Klicken Sie, um das Bild in voller Größe anzuzeigen](validation-with-the-data-annotation-validators-vb/_static/image14.png))

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial haben Sie gelernt, wie Sie den Modell Binder für die Daten Anmerkung nutzen können, um die Validierung innerhalb einer ASP.NET-MVC-Anwendung auszuführen. Sie haben gelernt, wie die verschiedenen Typen von Prüfungs-Attributen verwendet werden, z. b. die erforderlichen und StringLength-Attribute. Sie haben auch gelernt, wie diese Attribute bei der Arbeit mit dem Microsoft-Entity Framework verwendet werden.

> [!div class="step-by-step"]
> [Previous](validating-with-a-service-layer-vb.md)
