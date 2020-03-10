---
uid: web-api/overview/advanced/sending-html-form-data-part-2
title: 'Senden von HTML-Formulardaten in ASP.net-Web-API: Datei Upload und mehrteilige MIME-ASP.NET 4. x'
author: MikeWasson
description: In diesem Tutorial wird gezeigt, wie Sie Dateien in eine Web-API hochladen. Außerdem wird beschrieben, wie mehrteilige MIME-Daten verarbeitet werden.
ms.author: riande
ms.date: 06/21/2012
ms.custom: seoapril2019
ms.assetid: a7f3c1b5-69d9-4261-b082-19ffafa5f16a
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-2
msc.type: authoredcontent
ms.openlocfilehash: f5aaebb96f631dfb6b0da1fbca96cd93a6a7fe2d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449211"
---
# <a name="sending-html-form-data-in-aspnet-web-api-file-upload-and-multipart-mime"></a>Senden von HTML-Formulardaten in ASP.net-Web-API: Datei Upload und mehrteilige MIME

von [Mike Wasson](https://github.com/MikeWasson)

## <a name="part-2-file-upload-and-multipart-mime"></a>Teil 2: Datei Upload und mehrteilige MIME

In diesem Tutorial wird gezeigt, wie Sie Dateien in eine Web-API hochladen. Außerdem wird beschrieben, wie mehrteilige MIME-Daten verarbeitet werden.

> [!NOTE]
> [Laden Sie das abgeschlossene Projekt herunter](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).

Im folgenden finden Sie ein Beispiel für ein HTML-Formular zum Hochladen einer Datei:

[!code-html[Main](sending-html-form-data-part-2/samples/sample1.html)]

![](sending-html-form-data-part-2/_static/image1.png)

Dieses Formular enthält ein Texteingabe-und ein Dateieingabe-Steuerelement. Wenn ein Formular ein Dateieingabe-Steuerelement enthält, sollte das **enctype** -Attribut immer &quot;mehrteiligen/Form-Data-&quot;sein, das angibt, dass das Formular als mehrteilige MIME-Nachricht gesendet wird.

Das Format einer mehrteiligen MIME-Nachricht ist am einfachsten zu verstehen, indem Sie eine Beispiel Anforderung betrachten:

[!code-console[Main](sending-html-form-data-part-2/samples/sample2.cmd)]

Diese Meldung ist in zwei *Teile*unterteilt, eine für jedes Formular Steuerelement. Teile Grenzen werden durch die Zeilen angegeben, die mit Bindestrichen beginnen.

> [!NOTE]
> Die teilgrenze schließt eine zufällige Komponente (&quot;41184676334&quot;) ein, um sicherzustellen, dass die Begrenzungs Zeichenfolge nicht versehentlich in einem Nachrichten Teil angezeigt wird.

Jeder Nachrichten Teil enthält mindestens einen Header, gefolgt vom Teil Inhalt.

- Der Content-Disposition-Header enthält den Namen des Steuer Elements. Für Dateien enthält Sie auch den Dateinamen.
- Der Content-Type-Header beschreibt die Daten im-Teil. Wenn dieser Header weggelassen wird, ist der Standardwert Text/Plain.

Im vorherigen Beispiel hat der Benutzer eine Datei mit dem Namen "grandcanyon. jpg" mit dem Inhaltstyp "Image/JPEG;" hochgeladen. und der Wert der Texteingabe war &quot;Sommerurlaub&quot;.

## <a name="file-upload"></a>Dateiupload

Betrachten wir nun einen Web-API-Controller, der Dateien aus einer mehrteiligen MIME-Nachricht liest. Der Controller liest die Dateien asynchron. Die Web-API unterstützt asynchrone Aktionen mithilfe des [aufgabenbasierten Programmiermodells](https://msdn.microsoft.com/library/dd460693.aspx). Im folgenden finden Sie den Code, wenn Sie auf .NET Framework 4,5 abzielen, der die Schlüsselwörter **Async** und **erwartet** unterstützt.

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample3.cs)]

Beachten Sie, dass die Controller Aktion keine Parameter annimmt. Der Grund hierfür ist, dass der Anforderungs Text innerhalb der Aktion verarbeitet wird, ohne dass ein Formatierer für den Medientyp aufgerufen wird.

Die **ismultipartcontent** -Methode überprüft, ob die Anforderung eine mehrteilige MIME-Nachricht enthält. Andernfalls gibt der Controller den HTTP-Statuscode 415 (nicht unterstützter Medientyp) zurück.

Die **multipartformdatastreamprovider** -Klasse ist ein Hilfsobjekt, das Datei Datenströme für hochgeladene Dateien zuordnet. Um die mehrteilige MIME-Nachricht zu lesen, müssen Sie die Methode " **leseasmultipartasync** " aufrufen. Diese Methode extrahiert alle Nachrichten Teile und schreibt Sie in die Streams, die vom **multipartformdatastreamprovider**bereitgestellt werden.

Wenn die Methode abgeschlossen ist, können Sie Informationen zu den Dateien aus der **FileData** -Eigenschaft, die eine Sammlung von **multipartfiledata** -Objekten ist, erhalten.

- **Multipartfiledata. filename** ist der lokale Dateiname auf dem Server, auf dem die Datei gespeichert wurde.
- **Multipartfiledata. Headers** enthält den Part-Header (*nicht* den-Anforderungs Header). Damit können Sie auf die Content-\_Disposition-und Content-Type-Header zugreifen.

Wie der Name schon sagt, handelt es sich bei "read **asmultipartasync** " um eine asynchrone Methode. Um nach Abschluss der-Methode Aufgaben auszuführen, verwenden Sie eine [Fortsetzungs Aufgabe](https://msdn.microsoft.com/library/ee372288.aspx) (.NET 4,0) oder das **erwartet** -Schlüsselwort (.NET 4,5).

Im folgenden finden Sie die .NET Framework 4,0-Version des vorherigen Codes:

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample4.cs)]

## <a name="reading-form-control-data"></a>Lesen von Formular Steuerungsdaten

Das HTML-Formular, das ich zuvor gezeigt habe, hatte ein Texteingabe-Steuerelement.

[!code-html[Main](sending-html-form-data-part-2/samples/sample5.html)]

Sie können den Wert des Steuer Elements aus der **formdata** -Eigenschaft von **multipartformdatastreamprovider**erhalten.

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample6.cs?highlight=15)]

**Formdata** ist eine **NameValueCollection** , die Name-Wert-Paare für die Formular Steuerelemente enthält. Die Sammlung kann doppelte Schlüssel enthalten. Beachten Sie dieses Formular:

[!code-html[Main](sending-html-form-data-part-2/samples/sample7.html)]

![](sending-html-form-data-part-2/_static/image2.png)

Der Anforderungs Text könnte wie folgt aussehen:

[!code-console[Main](sending-html-form-data-part-2/samples/sample8.cmd)]

In diesem Fall enthält die **formdata** -Sammlung die folgenden Schlüssel-Wert-Paare:

- Fahrt: Roundtrip
- Optionen: nonstop
- Optionen: Datumsangaben
- Arbeitsplatz: Fenster
