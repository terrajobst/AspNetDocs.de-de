---
uid: web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
title: Validieren von Benutzereingaben in ASP.net Web Pages (Razor)-Sites | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Artikel wird erläutert, wie Sie Informationen überprüfen können, die Sie von Benutzern erhalten &mdash; d. h. um sicherzustellen, dass Benutzer gültige Informationen in HTML-Formularen in einem AS...
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 4eb060cc-cf14-41ae-bab1-14a2c15332d0
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: e6f8e1051d09d11f1756bfada44a73ba7c2a1db2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78454299"
---
# <a name="validating-user-input-in-aspnet-web-pages-razor-sites"></a>Überprüfen von Benutzereingaben in ASP.net Web Pages-Websites (Razor)

von [Tom fitzmacken](https://github.com/tfitzmac)

> In diesem Artikel wird erläutert, wie Sie Informationen überprüfen können, die Sie von Benutzern erhalten &mdash; das heißt, um sicherzustellen, dass Benutzer gültige Informationen in HTML-Formularen auf einer ASP.net Web Pages (Razor)-Website eingeben.
> 
> Sie lernen Folgendes:
> 
> - Überprüfen, ob die Eingabe eines Benutzers mit den von Ihnen definierten Validierungs Kriterien übereinstimmt.
> - Bestimmen, ob alle Validierungstests bestanden wurden.
> - Vorgehensweise beim Anzeigen von Validierungs Fehlern (und Formatierungs Fehlern).
> - Validieren von Daten, die nicht direkt von Benutzern stammen.
> 
> Dies sind die ASP.net-Programmier Konzepte, die im Artikel vorgestellt werden:
> 
> - Das `Validation`-Hilfsprogramm.
> - Die Methoden `Html.ValidationSummary` und `Html.ValidationMessage`.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Im Tutorial verwendete Software Versionen
> 
> 
> - ASP.net Web Pages (Razor) 3
>   
> 
> Dieses Tutorial funktioniert auch mit ASP.net Web Pages 2.

Dieser Artikel enthält folgende Abschnitte:

- [Übersicht über die Überprüfung der Benutzereingaben](#Overview_of_User_Input_Validation)
- [Überprüfen der Benutzereingabe](#Validating_User_Input)
- [Hinzufügen der Client seitigen Validierung](#Adding_Client-Side_Validation)
- [Formatieren von Validierungs Fehlern](#Formatting_Validation_Errors)
- [Validieren von Daten, die nicht direkt von Benutzern stammen](#Validating_Data_That_Doesnt_Come_Directly_from_Users)

<a id="Overview_of_User_Input_Validation"></a>
## <a name="overview-of-user-input-validation"></a>Übersicht über die Überprüfung der Benutzereingaben

Wenn Sie Benutzer dazu auffordern, Informationen in eine Seite einzugeben, z. –. in ein Formular – müssen Sie sicherstellen, dass die eingegebenen Werte gültig sind. Sie möchten z. b. nicht ein Formular verarbeiten, das wichtige Informationen fehlt.

Wenn Benutzer Werte in ein HTML-Formular eingeben, sind die Werte, die Sie eingeben, Zeichen folgen. In vielen Fällen sind die Werte, die Sie benötigen, einige andere Datentypen, wie z. b. ganze Zahlen oder Datumsangaben. Daher müssen Sie auch sicherstellen, dass die von Benutzern eingegebenen Werte ordnungsgemäß in die entsprechenden Datentypen konvertiert werden können.

Möglicherweise haben Sie auch bestimmte Einschränkungen für die Werte. Auch wenn Benutzer eine ganze Zahl ordnungsgemäß eingeben, müssen Sie möglicherweise sicherstellen, dass der Wert innerhalb eines bestimmten Bereichs liegt.

![Validierungs Fehler, die CSS-Stil Klassen verwenden](validating-user-input-in-aspnet-web-pages-sites/_static/image1.png)

> [!NOTE] 
> 
> **Wichtig** Die Überprüfung von Benutzereingaben ist auch wichtig für die Sicherheit. Wenn Sie die Werte einschränken, die Benutzer in Formularen eingeben können, verringern Sie die Wahrscheinlichkeit, dass ein Benutzer einen Wert eingeben kann, der die Sicherheit Ihrer Website beeinträchtigen kann.

<a id="Validating_User_Input"></a>
## <a name="validating-user-input"></a>Überprüfen der Benutzereingabe

In ASP.net Web Pages 2 können Sie das `Validator`-Hilfsprogramm verwenden, um Benutzereingaben zu testen. Die grundlegende Vorgehensweise besteht darin, die folgenden Schritte durchzuführen:

1. Bestimmen Sie, welche Eingabeelemente (Felder) Sie überprüfen möchten.

    In der Regel werden Werte in `<input>` Elementen in einem Formular überprüft. Es empfiehlt sich jedoch, alle Eingaben zu validieren, auch Eingaben, die von einem eingeschränkten Element wie einer `<select>` Liste stammen. Dadurch wird sichergestellt, dass die Benutzer die Steuerelemente auf einer Seite nicht umgehen und ein Formular übermitteln.
2. Fügen Sie im Seitencode individuelle Validierungs Überprüfungen für jedes Eingabe Element hinzu, indem Sie Methoden des `Validation` Hilfsprogramms verwenden.

    Um die erforderlichen Felder zu überprüfen, verwenden Sie `Validation.RequireField(field, [error message])` (für ein einzelnes Feld) oder `Validation.RequireFields(field1, field2, ...))` (für eine Liste von Feldern). Verwenden Sie für andere Validierungs Typen `Validation.Add(field, ValidationType)`. Für `ValidationType`können Sie die folgenden Optionen verwenden:

    `Validator.DateTime ([error message])`  
   `Validator.Decimal([error message])`  
   `Validator.EqualsTo(otherField [, error message])`  
   `Validator.Float([error message])`  
   `Validator.Integer([error message])`  
   `Validator.Range(min, max [, error message])`  
   `Validator.RegEx(pattern [, error message])`  
   `Validator.Required([error message])`  
   `Validator.StringLength(length)`  
   `Validator.Url([error message])`
3. Wenn die Seite übermittelt wird, überprüfen Sie, ob die Überprüfung erfolgreich war, indem Sie `Validation.IsValid`:

    [!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample1.cs)]

    Wenn Validierungs Fehler vorliegen, überspringen Sie die normale Seiten Verarbeitung. Wenn z. b. der Zweck der Seite darin besteht, eine Datenbank zu aktualisieren, tun Sie dies erst, wenn alle Validierungs Fehler behoben wurden.
4. Wenn Validierungs Fehler vorliegen, zeigen Sie die Fehlermeldungen im Markup der Seite an, indem Sie `Html.ValidationSummary` oder `Html.ValidationMessage`oder beides verwenden.

Das folgende Beispiel zeigt eine Seite, die diese Schritte veranschaulicht.

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample2.cshtml)]

Um zu sehen, wie die Validierung funktioniert, führen Sie diese Seite aus und führen absichtlich zu Fehlern. So sieht die Seite z. b. aus, wenn Sie vergessen, einen Kursnamen einzugeben, wenn Sie einen eingeben und ein ungültiges Datum eingeben:

![Validierungs Fehler auf der gerenderten Seite](validating-user-input-in-aspnet-web-pages-sites/_static/image2.png)

<a id="Adding_Client-Side_Validation"></a>
## <a name="adding-client-side-validation"></a>Hinzufügen der Client seitigen Validierung

Standardmäßig wird die Benutzereingabe überprüft, nachdem die Benutzer die Seite gesendet haben – das heißt, die Überprüfung wird im Servercode ausgeführt. Ein Nachteil dieses Ansatzes ist, dass die Benutzer wissen, dass Sie erst nach dem Senden der Seite einen Fehler ausgegeben haben. Wenn ein Formular lang oder komplex ist, kann es für den Benutzer unpraktisch sein, Fehler zu melden, nachdem die Seite übermittelt wurde.

Sie können Unterstützung hinzufügen, um die Validierung in Client Skripts auszuführen. In diesem Fall wird die Überprüfung ausgeführt, wenn Benutzer im Browser arbeiten. Angenommen, Sie geben an, dass ein Wert eine ganze Zahl sein soll. Wenn ein Benutzer einen nicht ganzzahligen Wert eingibt, wird der Fehler angezeigt, sobald der Benutzer das Eingabefeld verlässt. Benutzer erhalten sofortiges Feedback, was für Sie praktisch ist. Die Client basierte Validierung kann auch die Häufigkeit verringern, mit der der Benutzer das Formular einreichen muss, um mehrere Fehler zu beheben.

> [!NOTE]
> Auch wenn Sie die Client seitige Validierung verwenden, wird die Validierung immer auch im Servercode ausgeführt. Das Durchführen der Validierung in Servercode ist eine Sicherheitsmaßnahme, wenn Benutzer die Client basierte Validierung umgehen.

1. Registrieren Sie die folgenden JavaScript-Bibliotheken auf der Seite:  

    [!code-html[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample3.html)]

   Zwei der Bibliotheken können aus einem Content Delivery Network (CDN) geladen werden, sodass Sie nicht unbedingt auf dem Computer oder Server vorhanden sein müssen. Sie müssen jedoch über eine lokale Kopie von *jQuery. Validate. Unaufdring. js*verfügen. Wenn Sie noch nicht mit einer webmatrix-Vorlage (z. b. einer **Starter Site** ) arbeiten, die die Bibliothek enthält, erstellen Sie eine Web Pages-Website, die auf der **Starter Site**basiert. Kopieren Sie dann die *js* -Datei auf die aktuelle Website.
2. Fügen Sie im Markup für jedes Element, das Sie überprüfen, einen-`Validation.For(field)`hinzu. Diese Methode gibt Attribute aus, die von der Client seitigen Validierung verwendet werden. (Anstelle von tatsächlichem JavaScript-Code gibt die-Methode Attribute wie `data-val-...`aus. Diese Attribute unterstützen unaufdringliche Client Validierung, bei der jQuery verwendet wird, um die Arbeit zu erledigen.)

Auf der folgenden Seite wird gezeigt, wie Sie dem oben gezeigten Beispiel Client Validierungs Funktionen hinzufügen.

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample4.cshtml?highlight=35-39,51,61,71)]

Nicht alle Validierungs Überprüfungen werden auf dem Client ausgeführt. Insbesondere wird die Datentyp Validierung (Integer, Date usw.) nicht auf dem Client ausgeführt. Die folgenden Überprüfungen funktionieren sowohl auf dem Client als auch auf dem Server:

- `Required`
- `Range(minValue, maxValue)`
- `StringLength(maxLength[, minLength])`
- `Regex(pattern)`
- `EqualsTo(otherField)`

In diesem Beispiel funktioniert der Test für ein gültiges Datum nicht im Client Code. Der Test wird jedoch im Servercode ausgeführt.

<a id="Formatting_Validation_Errors"></a>
## <a name="formatting-validation-errors"></a>Formatieren von Validierungs Fehlern

Sie können steuern, wie Validierungs Fehler angezeigt werden, indem Sie CSS-Klassen definieren, die die folgenden reservierten Namen haben:

- [https://login.microsoftonline.com/consumers/](`field-validation-error`). Definiert die Ausgabe der `Html.ValidationMessage` Methode, wenn ein Fehler angezeigt wird.
- [https://login.microsoftonline.com/consumers/](`field-validation-valid`). Definiert die Ausgabe der `Html.ValidationMessage` Methode, wenn kein Fehler vorliegt.
- [https://login.microsoftonline.com/consumers/](`input-validation-error`). Definiert, wie `<input>` Elemente gerendert werden, wenn ein Fehler auftritt. (Sie können diese Klasse z. b. verwenden, um die Hintergrundfarbe einer &lt;Eingabe&gt;-Elements auf eine andere Farbe festzulegen, wenn der Wert ungültig ist.) Diese CSS-Klasse wird nur während der Client Validierung verwendet (in ASP.net Web Pages 2).
- [https://login.microsoftonline.com/consumers/](`input-validation-valid`). Definiert die Darstellung von `<input>` Elementen, wenn kein Fehler vorliegt.
- [https://login.microsoftonline.com/consumers/](`validation-summary-errors`). Definiert die Ausgabe der `Html.ValidationSummary` Methode, die eine Liste von Fehlern anzeigt.
- [https://login.microsoftonline.com/consumers/](`validation-summary-valid`). Definiert die Ausgabe der `Html.ValidationSummary` Methode, wenn kein Fehler vorliegt.

Der folgende `<style>` Block zeigt Regeln für Fehlerbedingungen an.

[!code-css[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample5.css)]

Wenn Sie diesen Stilblock in den Beispielseiten von weiter oben in diesem Artikel einfügen, sieht die Fehleranzeige wie in der folgenden Abbildung aus:

![Validierungs Fehler, die CSS-Stil Klassen verwenden](validating-user-input-in-aspnet-web-pages-sites/_static/image3.png)

> [!NOTE]
> Wenn Sie die Client Validierung nicht in ASP.net Web Pages 2 verwenden, haben die CSS-Klassen für die `<input>` Elemente (`input-validation-error` und `input-validation-valid` keine Auswirkung.

### <a name="static-and-dynamic-error-display"></a>Statische und dynamische Fehleranzeige

Die CSS-Regeln treten paarweise auf, z. b. `validation-summary-errors` und `validation-summary-valid`. Mit diesen Paaren können Sie Regeln für beide Bedingungen definieren: eine Fehlerbedingung und eine "normale" Bedingung (nicht fehlerhaft). Es ist wichtig zu verstehen, dass das Markup für die Fehleranzeige immer gerendert wird, auch wenn keine Fehler vorliegen. Wenn eine Seite z. b. über eine `Html.ValidationSummary`-Methode im Markup verfügt, enthält die Seitenquelle das folgende Markup, auch wenn die Seite zum ersten Mal angefordert wird:

`<div class="validation-summary-valid" data-valmsg-summary="true"><ul></ul></div>`

Mit anderen Worten, die `Html.ValidationSummary`-Methode rendert immer ein `<div>` Element und eine Liste, auch wenn die Fehlerliste leer ist. Ebenso rendert die `Html.ValidationMessage`-Methode immer ein `<span>`-Element als Platzhalter für einen einzelnen Feld Fehler, auch wenn kein Fehler vorliegt.

In einigen Situationen kann das Anzeigen einer Fehlermeldung bewirken, dass die Seite erneut fließt und die Elemente auf der Seite verschoben werden können. Die CSS-Regeln, die auf `-valid` enden, ermöglichen es Ihnen, ein Layout zu definieren, das zur Vermeidung dieses Problems beiträgt. Beispielsweise können Sie `field-validation-error` und `field-validation-valid` definieren, sodass beide die gleiche festgelegte Größe aufweisen. Auf diese Weise ist der Anzeigebereich für das Feld statisch und ändert den Seiten Fluss nicht, wenn eine Fehlermeldung angezeigt wird.

<a id="Validating_Data_That_Doesnt_Come_Directly_from_Users"></a>
## <a name="validating-data-that-doesnt-come-directly-from-users"></a>Validieren von Daten, die nicht direkt von Benutzern stammen

Manchmal müssen Sie Informationen überprüfen, die nicht direkt aus einem HTML-Formular stammen. Ein typisches Beispiel ist eine Seite, bei der ein Wert in einer Abfrage Zeichenfolge wie im folgenden Beispiel dargestellt wird:

`http://server/myapp/EditClassInformation?classid=1022`

In diesem Fall müssen Sie sicherstellen, dass der Wert, der an die Seite (hier 1022 für den Wert von `classid`), an die Seite übermittelt wird, gültig ist. Sie können das `Validation`-Hilfsprogramm nicht direkt verwenden, um diese Validierung auszuführen. Sie können jedoch auch andere Funktionen des validierungssystems verwenden, z. b. die Möglichkeit, Validierungs Fehlermeldungen anzuzeigen.

> [!NOTE] 
> 
> **Wichtig** Überprüfen Sie immer die Werte, die Sie aus *beliebigen* Quellen erhalten, einschließlich Formular Feldwerten, Werte von Abfrage Zeichenfolgen und Cookie-Werten. Es ist einfach, dass die Benutzer diese Werte ändern können (vielleicht für böswillige Zwecke). Sie müssen diese Werte also überprüfen, um Ihre Anwendung zu schützen.

Im folgenden Beispiel wird gezeigt, wie Sie einen Wert validieren können, der in einer Abfrage Zeichenfolge übermittelt wird. Der Code testet, ob der Wert nicht leer ist und eine ganze Zahl ist.

[!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample6.cs)]

Beachten Sie, dass der Test ausgeführt wird, wenn es sich bei der Anforderung nicht um eine Formular Übermittlung (`if(!IsPost)`) handelt. Dieser Test wird bei der ersten Anforderung der Seite bestanden, jedoch nicht, wenn es sich bei der Anforderung um eine Formular Übermittlung handelt.

Zum Anzeigen dieses Fehlers können Sie den Fehler der Liste der Validierungs Fehler hinzufügen, indem Sie `Validation.AddFormError("message")`aufrufen. Wenn die Seite einen aufzurufenden `Html.ValidationSummary` Methode enthält, wird der Fehler an dieser Stelle angezeigt, genau wie bei einem Validierungs Fehler bei der Benutzereingabe.

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Arbeiten mit HTML-Formularen an ASP.net Web Pages Websites](https://go.microsoft.com/fwlink/?LinkID=202892)
