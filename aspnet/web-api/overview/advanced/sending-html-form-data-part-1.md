---
uid: web-api/overview/advanced/sending-html-form-data-part-1
title: 'Senden von HTML-Formulardaten in ASP.net-Web-API: form-urlencoded Data-ASP.NET 4. x'
author: MikeWasson
description: In diesem Artikel wird gezeigt, wie Sie form-urlencoded-Daten an einen Web-API-Controller mit ASP.NET 4. x senden.
ms.author: riande
ms.date: 06/15/2012
ms.custom: seoapril2019
ms.assetid: 585351c4-809a-4bf5-bcbe-35d624f565fe
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-1
msc.type: authoredcontent
ms.openlocfilehash: 7243069dbd8051b1374ed6e0112c273b8fe26f61
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449241"
---
# <a name="sending-html-form-data-in-aspnet-web-api-form-urlencoded-data"></a>Senden von HTML-Formulardaten in ASP.net-Web-API: Formular-urlencoded-Daten

von [Mike Wasson](https://github.com/MikeWasson)

## <a name="part-1-form-urlencoded-data"></a>Teil 1: Formular-urlencoded-Daten

In diesem Artikel wird gezeigt, wie Sie in Form von urlencoded-Daten an einen Web-API-Controller Posten.

- [Übersicht über HTML-Formulare](#overview_of_html_forms)
- [Senden komplexer Typen](#sending_complex_types)
- [Senden von Formulardaten über AJAX](#sending_form_data_via_ajax)
- [Senden von einfachen Typen](#sending_simple_types)

> [!NOTE]
> [Laden Sie das abgeschlossene Projekt herunter](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).

<a id="overview_of_html_forms"></a>
## <a name="overview-of-html-forms"></a>Übersicht über HTML-Formulare

HTML-Formulare verwenden entweder Get oder Post zum Senden von Daten an den Server. Das **method** -Attribut des **Form** -Elements gibt die HTTP-Methode an:

[!code-html[Main](sending-html-form-data-part-1/samples/sample1.html)]

Die Standardmethode ist "Get". Wenn das Formular Get verwendet, werden die Formulardaten im URI als Abfrage Zeichenfolge codiert. Wenn das Formular Post verwendet, werden die Formulardaten im Anforderungs Text abgelegt. Für veröffentlichte Daten gibt das **enctype** -Attribut das Format des Anforderungs Texts an:

| "CType" | Beschreibung |
| --- | --- |
| application/x-www-form-urlencoded | Formulardaten werden als Name-Wert-Paare codiert, ähnlich wie eine URI-Abfrage Zeichenfolge. Dies ist das Standardformat für Post. |
| Multipart/Form-Data | Formulardaten werden als mehrteilige MIME-Nachricht codiert. Verwenden Sie dieses Format, wenn Sie eine Datei auf den Server hochladen. |

In Teil 1 dieses Artikels wird das Format "x-www-form-urlencoded" untersucht. [Teil 2](sending-html-form-data-part-2.md) beschreibt mehrteilige MIME.

<a id="sending_complex_types"></a>
## <a name="sending-complex-types"></a>Senden komplexer Typen

Normalerweise senden Sie einen komplexen Typ, der aus Werten besteht, die von verschiedenen Formular Steuerelementen entnommen werden. Beachten Sie das folgende Modell, das eine Statusaktualisierung darstellt:

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample2.cs)]

Hier ist ein Web-API-Controller, der eine `Update` Objekt per Post akzeptiert.

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample3.cs)]

> [!NOTE]
> Dieser Controller verwendet [Aktions basiertes Routing](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), sodass die Routen Vorlage &quot;API/{Controller}/{Action}/{ID}&quot;ist. Der Client sendet die Daten an &quot;/API/Updates/Complex-&quot;.

Nun schreiben wir ein HTML-Formular, damit Benutzer eine Statusaktualisierung übermitteln können.

[!code-html[Main](sending-html-form-data-part-1/samples/sample4.html)]

Beachten Sie, dass das **Action** -Attribut im Formular der URI der Controller Aktion ist. Hier ist das Formular, in das einige Werte eingegeben werden:

![](sending-html-form-data-part-1/_static/image1.png)

Wenn der Benutzer auf Senden klickt, sendet der Browser eine HTTP-Anforderung ähnlich der folgenden:

[!code-console[Main](sending-html-form-data-part-1/samples/sample5.cmd)]

Beachten Sie, dass der Anforderungs Text die Formulardaten enthält, die als Name-Wert-Paare formatiert sind. Die Web-API konvertiert die Name-Wert-Paare automatisch in eine Instanz der `Update`-Klasse.

<a id="sending_form_data_via_ajax"></a>
## <a name="sending-form-data-via-ajax"></a>Senden von Formulardaten über AJAX

Wenn ein Benutzer ein Formular sendet, navigiert der Browser von der aktuellen Seite Weg und rendert den Text der Antwortnachricht. Das ist in Ordnung, wenn die Antwort eine HTML-Seite ist. Bei einer Web-API ist der Antworttext jedoch in der Regel entweder leer oder enthält strukturierte Daten, wie z. b. JSON. In diesem Fall ist es sinnvoller, die Formulardaten mit einer AJAX-Anforderung zu senden, damit die Seite die Antwort verarbeiten kann.

Der folgende Code zeigt, wie Sie Formulardaten mithilfe von jQuery veröffentlichen.

[!code-html[Main](sending-html-form-data-part-1/samples/sample6.html)]

Die jQuery-Funktion zum **senden** ersetzt die Form-Aktion durch eine neue Funktion. Dies überschreibt das Standardverhalten der Schaltfläche "Senden". Die **serialisieren** -Funktion serialisiert die Formulardaten in Name/Wert-Paare. Um die Formulardaten an den Server zu senden, wenden Sie `$.post()`an.

Wenn die Anforderung abgeschlossen ist, zeigt der `.success()` oder `.error()` Handler dem Benutzer eine entsprechende Meldung an.

![](sending-html-form-data-part-1/_static/image2.png)

<a id="sending_simple_types"></a>
## <a name="sending-simple-types"></a>Senden von einfachen Typen

In den vorherigen Abschnitten haben wir einen komplexen Typ gesendet, der die Web-API in eine Instanz einer Modell Klasse deserialisiert hat. Sie können auch einfache Typen, z. b. eine Zeichenfolge, senden.

> [!NOTE]
> Vor dem Senden eines einfachen Typs sollten Sie den Wert stattdessen in einem komplexen Typ umhüllen. Dies bietet Ihnen die Vorteile der Modell Validierung auf Serverseite und erleichtert das Erweitern des Modells bei Bedarf.

Die grundlegenden Schritte zum Senden eines einfachen Typs sind identisch, aber es gibt zwei feine Unterschiede. Zuerst müssen Sie im Controller den Parameternamen mit dem **frombody** -Attribut ergänzen.

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample7.cs?highlight=3)]

Standardmäßig versucht die Web-API, einfache Typen aus dem Anforderungs-URI zu erhalten. Das **frombody** -Attribut weist die Web-API an, den Wert aus dem Anforderungs Text zu lesen.

> [!NOTE]
> Die Web-API liest den Antworttext höchstens einmal, sodass nur ein Parameter einer Aktion aus dem Anforderungs Text stammen kann. Wenn Sie mehrere Werte aus dem Anforderungs Text erhalten müssen, definieren Sie einen komplexen Typ.

Zweitens muss der Client den Wert im folgenden Format senden:

[!code-xml[Main](sending-html-form-data-part-1/samples/sample8.xml)]

Insbesondere muss der Namensteil des Name-Wert-Paars für einen einfachen Typ leer sein. Nicht alle Browser unterstützen dies für HTML-Formulare, aber Sie erstellen dieses Format wie folgt im Skript:

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample9.js)]

Im folgenden finden Sie ein Beispiel für eine Form:

[!code-html[Main](sending-html-form-data-part-1/samples/sample10.html)]

Und hier ist das Skript zum Übermitteln des Formular Werts. Der einzige Unterschied zum vorherigen Skript ist das Argument, das an die **Post** -Funktion übermittelt wird.

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample11.js?highlight=2)]

Sie können den gleichen Ansatz verwenden, um ein Array einfacher Typen zu senden:

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample12.js)]

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Teil 2: Datei Upload und mehrteilige MIME](sending-html-form-data-part-2.md)
