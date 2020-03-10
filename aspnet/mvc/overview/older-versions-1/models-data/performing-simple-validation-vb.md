---
uid: mvc/overview/older-versions-1/models-data/performing-simple-validation-vb
title: Ausführen einer einfachen Überprüfung (VB) | Microsoft-Dokumentation
author: StephenWalther
description: Erfahren Sie, wie Sie eine Validierung in einer ASP.NET MVC-Anwendung ausführen. In diesem Tutorial führt Stephen Walther Sie in den Modell Status und das Validierungs-HTML-Hilfsprogramm ein...
ms.author: riande
ms.date: 03/02/2009
ms.assetid: df6cf4b7-0bb3-4c4e-b17a-bd78a759a6bc
msc.legacyurl: /mvc/overview/older-versions-1/models-data/performing-simple-validation-vb
msc.type: authoredcontent
ms.openlocfilehash: 46925f22b7dfc23f2bb89b8d2fff0cbd8ae49062
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78436665"
---
# <a name="performing-simple-validation-vb"></a>Ausführen einer einfachen Überprüfung (VB)

von [Stephen Walther](https://github.com/StephenWalther)

> Erfahren Sie, wie Sie eine Validierung in einer ASP.NET MVC-Anwendung ausführen. In diesem Tutorial führt Stephen Walther Sie in den Modell Status und die Validierungs-HTML-Hilfsprogramme ein.

In diesem Tutorial wird erläutert, wie Sie die Validierung innerhalb einer ASP.NET MVC-Anwendung ausführen können. Beispielsweise erfahren Sie, wie Sie verhindern können, dass jemand ein Formular übermittelt, das keinen Wert für ein Pflichtfeld enthält. Sie erfahren, wie Sie den Modell Status und die Validierungs-HTML-Hilfsprogramme verwenden.

## <a name="understanding-model-state"></a>Grundlegendes zum Modell Status

Sie verwenden den Modell Status oder genauer, das Modell Zustands Wörterbuch zum Darstellen von Validierungs Fehlern. Die Create ()-Aktion in der Liste 1 überprüft beispielsweise die Eigenschaften einer Product-Klasse, bevor die Product-Klasse einer Datenbank hinzugefügt wird.

Ich empfehle Ihnen nicht, die Validierung oder Daten Bank Logik einem Controller hinzuzufügen. Ein Controller sollte nur Logik im Zusammenhang mit der Steuerung des Anwendungs Flusses enthalten. Wir verwenden eine Verknüpfung, um die Dinge einfach zu halten.

**Codebeispiel 1-controllers\productcontroller.vb**

[!code-vb[Main](performing-simple-validation-vb/samples/sample1.vb)]

In der Liste 1 werden die Eigenschaften Name, Description und UnitsInStock der Product-Klasse überprüft. Wenn eine dieser Eigenschaften einen Überprüfungs Test nicht bestanden hat, wird dem Modell Zustands Wörterbuch ein Fehler hinzugefügt (dargestellt durch die modelstate-Eigenschaft der Controller-Klasse).

Wenn im Modell Status Fehler auftreten, gibt die modelstate. IsValid-Eigenschaft den Wert false zurück. In diesem Fall wird das HTML-Formular zum Erstellen eines neuen Produkts erneut angezeigt. Andernfalls wird das neue Produkt der Datenbank hinzugefügt, wenn keine Validierungs Fehler vorliegen.

## <a name="using-the-validation-helpers"></a>Verwenden der Validierungs Hilfsprogramme

Das ASP.NET-MVC-Framework enthält zwei Validierungs Hilfen: das Hilfsprogramm HTML. validationmessage () und das Hilfsprogramm HTML. ValidationSummary (). Sie verwenden diese beiden Hilfsprogramme in einer Ansicht, um Validierungs Fehlermeldungen anzuzeigen.

Die HTML. validationmessage ()-und HTML. ValidationSummary ()-Hilfsprogramme werden in den Erstellungs-und Bearbeitungs Sichten verwendet, die automatisch vom ASP.NET-MVC-Gerüstbau generiert werden. Führen Sie die folgenden Schritte aus, um die CREATE VIEW zu generieren:

1. Klicken Sie mit der rechten Maustaste auf die Aktion erstellen () im Product Controller, und wählen Sie die Menüoption **Ansicht hinzufügen** (siehe Abbildung 1) aus.
2. Aktivieren Sie im Dialog **Feld Ansicht hinzufügen** das Kontrollkästchen mit der Bezeichnung **eine stark typisierte Ansicht erstellen** (siehe Abbildung 2).
3. Wählen Sie in der Dropdown Liste **Datenklasse anzeigen** die Product-Klasse aus.
4. Wählen Sie in der Dropdown Liste **Inhalt anzeigen** die Option erstellen aus.
5. Klicken Sie auf die Schaltfläche **Hinzufügen** .

Stellen Sie sicher, dass Sie die Anwendung erstellen, bevor Sie eine Ansicht hinzufügen. Andernfalls wird die Liste der Klassen nicht in der Dropdown Liste **Datenklasse anzeigen** angezeigt.

[![des Dialog Felds "Neues Projekt"](performing-simple-validation-vb/_static/image1.jpg)](performing-simple-validation-vb/_static/image1.png)

**Abbildung 01**: Hinzufügen einer Ansicht ([Klicken Sie, um das Bild in voller Größe anzuzeigen](performing-simple-validation-vb/_static/image2.png))

[![des Dialog Felds "Neues Projekt"](performing-simple-validation-vb/_static/image2.jpg)](performing-simple-validation-vb/_static/image3.png)

**Abbildung 02**: Erstellen einer stark typisierten Ansicht ([Klicken Sie, um das Bild in voller Größe anzuzeigen](performing-simple-validation-vb/_static/image4.png))

Nachdem Sie diese Schritte ausgeführt haben, erhalten Sie die Ansicht erstellen in der Liste 2.

**Codebeispiel 2: views\product\kreate.aspx**

[!code-aspx[Main](performing-simple-validation-vb/samples/sample2.aspx)]

In der Liste 2 wird das Hilfsprogramm HTML. ValidationSummary () direkt über dem HTML-Formular aufgerufen. Dieses Hilfsprogramm wird verwendet, um eine Liste von Validierungs Fehlermeldungen anzuzeigen. Das Hilfsprogramm HTML. ValidationSummary () rendert die Fehler in einer Auflistungs Liste.

Das Hilfsprogramm HTML. validationmessage () wird neben den einzelnen HTML-Formularfeldern aufgerufen. Dieses Hilfsprogramm wird verwendet, um direkt neben einem Formularfeld eine Fehlermeldung anzuzeigen. Im Fall von "Listing 2" zeigt das Hilfsprogramm HTML. validationmessage () ein Sternchen an, wenn ein Fehler vorliegt.

Die Seite in Abbildung 3 veranschaulicht die Fehlermeldungen, die von den Validierungs Hilfen gerendert werden, wenn das Formular mit fehlenden Feldern und ungültigen Werten übermittelt wird.

[![des Dialog Felds "Neues Projekt"](performing-simple-validation-vb/_static/image3.jpg)](performing-simple-validation-vb/_static/image5.png)

**Abbildung 03**: die Erstellungs Ansicht wurde mit Problemen übermittelt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](performing-simple-validation-vb/_static/image6.png))

Beachten Sie, dass die Darstellung der HTML-Eingabefelder ebenfalls geändert wird, wenn ein Validierungs Fehler vorliegt. Das Hilfsprogramm HTML. TextBox () rendert ein *Class = "Input-Validation-Error"-* Attribut, wenn ein Validierungs Fehler vorliegt, der der Eigenschaft zugeordnet ist, die vom HTML. TextBox-Hilfsprogramm () gerendert wird.

Es gibt drei Cascading Stylesheet-Klassen, die verwendet werden, um die Darstellung von Validierungs Fehlern zu steuern:

- Input-Validation-Error-wird auf die &lt;Eingabe&gt;-Tags angewendet, das vom HTML. TextBox ()-Hilfsprogramm gerendert wird.
- Field-Validation-Error-wird auf die &lt;Spanne&gt; Tag angewendet, das vom HTML. validationmessage ()-Hilfsprogramm gerendert wird.
- Validation-Summary-Errors-wird auf das &lt;UL&gt;-Tag angewendet, das vom HTML. ValidationSummary ()-Hilfsprogramm gerendert wird.

Sie können diese Cascading Stylesheet-Klassen ändern und daher die Darstellung der Validierungs Fehler ändern, indem Sie die Datei "Site. CSS" ändern, die sich im Ordner "Content" befindet.

> [!NOTE] 
> 
> Die htmlhelper-Klasse enthält schreibgeschützte statische Eigenschaften zum Abrufen der Namen der Validierungs bezogenen CSS-Klassen. Diese statischen Eigenschaften heißen validationinputcssclassname, validationfieldcssclassname und validationsummarycssclassname.

## <a name="prebinding-validation-and-postbinding-validation"></a>Überprüfung der Vorbindung und postbinding-Überprüfung

Wenn Sie das HTML-Formular zum Erstellen eines Produkts übermitteln und einen ungültigen Wert für das Feld Price und keinen Wert für das Feld UnitsInStock eingeben, werden die in Abbildung 4 angezeigten Validierungs Meldungen angezeigt. Woher kommen diese Validierungs Fehlermeldungen?

[![des Dialog Felds "Neues Projekt"](performing-simple-validation-vb/_static/image4.jpg)](performing-simple-validation-vb/_static/image7.png)

**Abbildung 04**: vorbinden von Validierungs Fehlern ([Klicken Sie, um das Bild in voller Größe anzuzeigen](performing-simple-validation-vb/_static/image8.png))

Es gibt zwei Arten von Validierungs Fehlermeldungen: solche, die vor dem Erstellen der HTML-Formularfelder generiert werden, werden an eine Klasse gebunden, und diejenigen, die generiert werden, nachdem die Formularfelder an die Klasse gebunden wurden. Dies bedeutet, dass es vorbindungs-Validierungs Fehler und postbinding-Validierungs Fehler gibt.

Die Create ()-Aktion, die vom Produkt Controller in der Liste 1 verfügbar gemacht wird, akzeptiert eine Instanz der Product-Klasse. Die Signatur der Create-Methode sieht wie folgt aus:

[!code-vb[Main](performing-simple-validation-vb/samples/sample3.vb)]

Die Werte der HTML-Formularfelder aus dem Erstellungs Formular werden durch etwas, das als Modell Binder bezeichnet wird, an die productto Create-Klasse gebunden. Der Standardmodell Binder fügt eine Fehlermeldung hinzu, um den Status automatisch zu modellieren, wenn ein Formularfeld nicht an eine Formular Eigenschaft gebunden werden kann.

Der Standardmodell Binder kann die Zeichenfolge "Apple" nicht an die Price-Eigenschaft der Product-Klasse binden. Sie können einer Decimal-Eigenschaft keine Zeichenfolge zuweisen. Aus diesem Grund fügt der Modell Binder dem Modell Status einen Fehler hinzu.

Der Standardmodell Binder kann nicht den Wert Nothing einer Eigenschaft zuweisen, die den Wert Nothing nicht akzeptiert. Insbesondere kann der Modell Binder dem Wert "UnitsInStock" den Wert "Nothing" nicht zuweisen. Auch hier gibt der Modell Binder einen Fehler aus und fügt dem Modell Status eine Fehlermeldung hinzu.

Wenn Sie die Darstellung dieser vorbindungs Fehlermeldungen anpassen möchten, müssen Sie für diese Nachrichten Ressourcen Zeichenfolgen erstellen.

## <a name="summary"></a>Zusammenfassung

Ziel dieses Tutorials war es, die grundlegenden Mechanismen der Validierung im ASP.NET MVC-Framework zu beschreiben. Sie haben gelernt, wie Sie den Modell Status und die Validierungs-HTML-Hilfsprogramme verwenden. Außerdem wurde der Unterschied zwischen der Überprüfung vor und nach der Bereitstellung erläutert. In anderen Tutorials werden verschiedene Strategien zum Verschieben des Validierungs Codes von ihren Controllern und in Ihre Modellklassen erläutert.

> [!div class="step-by-step"]
> [Zurück](displaying-a-table-of-database-data-vb.md)
> [Weiter](validating-with-the-idataerrorinfo-interface-vb.md)
