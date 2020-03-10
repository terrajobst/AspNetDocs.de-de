---
uid: whitepapers/request-validation
title: Anforderungs Validierung-verhindern von Skript Angriffen | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Dokument wird das Anforderungs Überprüfungs Feature von ASP.net beschrieben, bei dem die Anwendung standardmäßig daran gehindert wird, nicht codierten HTML-Inhaltstyp zu verarbeiten...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: fa429113-5f8f-4ef4-97c5-5c04900a19fa
msc.legacyurl: /whitepapers/request-validation
msc.type: content
ms.openlocfilehash: 807cccd6fe1acdd6359b014387abd3878840d4cd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78520581"
---
# <a name="request-validation---preventing-script-attacks"></a>Anforderungsvalidierung – Schutz vor Angriffen durch Skripteinschleusung

> In diesem Dokument wird das Anforderungs Überprüfungs Feature von ASP.net beschrieben, wobei die Anwendung standardmäßig daran gehindert wird, nicht codierten HTML-Inhalt zu verarbeiten, der an den Server gesendet wurde. Diese Anforderungs Validierungsfunktion kann deaktiviert werden, wenn die Anwendung für die sichere Verarbeitung von HTML-Daten konzipiert wurde.
> 
> Gilt für ASP.NET 1,1 und ASP.NET 2,0.

Das Anforderungsüberprüfungsfeature von ASP.NET ist seit Version 1.1 verfügbar und verhindert, dass der Server Inhalte mit nicht codierter HTML akzeptiert. Dieses Feature dient zur Abwehr von Skripteinschleusungsangriffen, bei denen Clientskriptcode oder HTML unbewusst an einen Server übermittelt, gespeichert und anschließend anderen Benutzern angezeigt werden kann. Es wird weiterhin dringend empfohlen, sämtliche Eingabedaten zu überprüfen und gegebenenfalls als HTML zu codieren.

Beispielsweise erstellen Sie eine Webseite, die die e-Mail-Adresse eines Benutzers anfordert und diese e-Mail-Adresse dann in einer Datenbank speichert. Wenn der Benutzer &lt;Skript&gt;Warnung ("Hello from Script")&lt;/Script&gt; anstelle einer gültigen e-Mail-Adresse eingibt, kann dieses Skript ausgeführt werden, wenn der Inhalt nicht ordnungsgemäß codiert wurde. Das Anforderungs Validierungs Feature von ASP.net verhindert, dass dies geschieht.

## <a name="why-this-feature-is-useful"></a>Warum diese Funktion nützlich ist

Viele Websites sind nicht darauf aufmerksam, dass Sie für einfache Skript einschleusungs Angriffe offen sind. Unabhängig davon, ob sich der Zweck dieser Angriffe auf die Website durch Anzeigen von HTML oder das Ausführen eines Client Skripts zum Umleiten des Benutzers an die Website eines Hackers auswirken soll, sind Skript einschleusungs Angriffe ein Problem, mit dem sich Webentwickler auseinandersetzen müssen.

Skript einschleusungs Angriffe sind für alle Webentwickler von Bedeutung, unabhängig davon, ob Sie ASP.net, ASP oder andere Webentwicklungs Technologien verwenden.

Das ASP.net Request Validation-Feature verhindert diese Angriffe proaktiv, indem es nicht zulässt, dass nicht codierter HTML-Inhalt vom Server verarbeitet wird, es sei denn, der Entwickler beschließt, diesen Inhalt zuzulassen.

## <a name="what-to-expect-error-page"></a>Was Sie erwartet: Fehlerseite

Der nachfolgende Screenshot zeigt einen Beispielcode für ASP.net:

![](request-validation/_static/image1.png)

Das Ausführen dieses Codes führt zu einer einfachen Seite, die es Ihnen ermöglicht, Text in das Textfeld einzugeben, auf die Schaltfläche zu klicken und den Text im Label-Steuerelement anzuzeigen:

![](request-validation/_static/image2.png)

Wenn jedoch JavaScript, z. b. `<script>alert("hello!")</script>` eingegeben und gesendet werden, wird eine Ausnahme ausgelöst:

![](request-validation/_static/image3.png)

In der Fehlermeldung wird angegeben, dass ein potenziell gefährlicher Request. Form-Wert erkannt wurde, und es werden weitere Details in der Beschreibung angezeigt, um genau zu erkennen, was passiert ist und wie das Verhalten geändert werden kann. Beispiel:

Die Anforderungs Validierung hat einen potenziell gefährlichen Client Eingabe Wert festgestellt, und die Verarbeitung der Anforderung wurde abgebrochen. Dieser Wert kann auf einen Versuch hindeuten, die Sicherheit Ihrer Anwendung zu kompromittieren, z. b. ein Site übergreifender Scripting-Angriff. Sie können die Anforderungs Validierung deaktivieren, indem Sie `validateRequest=false` in der Page-Direktive oder im Konfigurations Abschnitt festlegen. Es wird jedoch dringend empfohlen, dass die Anwendung in diesem Fall explizit alle Eingaben überprüft.

## <a name="disabling-request-validation-on-a-page"></a>Deaktivieren der Anforderungs Validierung auf einer Seite

Um die Anforderungs Validierung auf einer Seite zu deaktivieren, müssen Sie das `validateRequest`-Attribut der Page-Direktive auf `false`festlegen:

[!code-aspx[Main](request-validation/samples/sample1.aspx)]

> [!CAUTION]
> Wenn die Anforderungs Validierung deaktiviert ist, kann der Inhalt an eine Seite übermittelt werden. Es liegt in der Verantwortung des Seiten Entwicklers, sicherzustellen, dass Inhalte ordnungsgemäß codiert oder verarbeitet werden.

## <a name="disabling-request-validation-for-your-application"></a>Deaktivieren der Anforderungs Validierung für Ihre Anwendung

Um die Anforderungs Validierung für die Anwendung zu deaktivieren, müssen Sie eine Web. config-Datei für Ihre Anwendung ändern oder erstellen und das ValidateRequest-Attribut des `<pages />` Abschnitts auf `false`festlegen:

[!code-xml[Main](request-validation/samples/sample2.xml)]

Wenn Sie die Anforderungs Validierung für alle Anwendungen auf dem Server deaktivieren möchten, können Sie diese Änderung an der Datei Machine. config vornehmen.

> [!CAUTION]
> Wenn die Anforderungs Validierung deaktiviert ist, kann der Inhalt an die Anwendung übermittelt werden. Es liegt in der Verantwortung des Anwendungs Entwicklers sicherzustellen, dass Inhalte ordnungsgemäß codiert oder verarbeitet werden.

Der folgende Code wird geändert, um die Anforderungs Validierung zu deaktivieren:

![](request-validation/_static/image4.png)

Wenn das folgende JavaScript in das Textfeld eingegeben wurde `<script>alert("hello!")</script>` lautet das Ergebnis wie folgt:

![](request-validation/_static/image5.png)

Um dies zu verhindern, muss der Inhalt bei deaktivierter Anforderungs Validierung in HTML-Code codiert werden.

## <a name="how-to-html-encode-content"></a>HTML-codieren von Inhalten

Wenn Sie die Anforderungs Validierung deaktiviert haben, empfiehlt es sich, Inhalte zu codieren, die für die zukünftige Verwendung gespeichert werden. Bei der HTML-Codierung werden alle '&lt;' oder '&gt;' (in Verbindung mit mehreren anderen Symbolen) automatisch durch die entsprechende HTML-codierte Darstellung ersetzt. Beispielsweise wird '&lt;' durch '&amp;lt; ' ersetzt, und '&gt;' wird durch '&amp;gt; ' ersetzt. Browser verwenden diese speziellen Codes, um "&lt;" oder "&gt;" im Browser anzuzeigen.

Der Inhalt kann auf dem Server auf einfache Weise auf dem Server mit der `Server.HtmlEncode(string)`-API codiert werden. Der Inhalt kann auch einfach HTML-decodiert werden, d. h. mit der `Server.HtmlDecode(string)`-Methode auf Standard-HTML zurückgesetzt werden.

![](request-validation/_static/image6.png)

Ergebnis:

![](request-validation/_static/image7.png)
