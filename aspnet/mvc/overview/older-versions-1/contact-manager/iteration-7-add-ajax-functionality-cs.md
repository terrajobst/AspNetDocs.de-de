---
uid: mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-cs
title: 'Iterations #7 – Hinzufügen von AJAXC#-Funktionen () | Microsoft-Dokumentation'
author: microsoft
description: In der siebten Iterationen verbessern wir die Reaktionsfähigkeit und Leistung unserer Anwendung durch Hinzufügen von Unterstützung für AJAX.
ms.author: riande
ms.date: 02/20/2009
ms.assetid: f1b0809e-8909-444e-b6bb-a5cd1dea3f72
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-cs
msc.type: authoredcontent
ms.openlocfilehash: 7c8eb3d3688674dd2c220b4bd1b5982f2610d0eb
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78437733"
---
# <a name="iteration-7--add-ajax-functionality-c"></a>Iterations #7 – Hinzufügen von AJAXC#-Funktionen ()

von [Microsoft](https://github.com/microsoft)

[Code herunterladen](iteration-7-add-ajax-functionality-cs/_static/contactmanager_7_cs1.zip)

> In der siebten Iterationen verbessern wir die Reaktionsfähigkeit und Leistung unserer Anwendung durch Hinzufügen von Unterstützung für AJAX.

## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Aufbauen einer Kontakt Verwaltung ASP.NET MVC-AnwendungC#()

In dieser Reihe von Tutorials erstellen wir von Anfang bis Ende eine gesamte Kontakt Verwaltungs Anwendung. Mithilfe der Kontakt-Manager-Anwendung können Sie Kontaktinformationen (Namen, Telefonnummern und e-Mail-Adressen) für eine Liste von Personen speichern.

Die Anwendung wird über mehrere Iterationen erstellt. Bei jeder Iterationen verbessern wir die Anwendung allmählich. Das Ziel dieses mehrfaches Iterations Ansatzes besteht darin, Ihnen zu ermöglichen, den Grund für jede Änderung zu verstehen.

- Iterations #1: Erstellen Sie die Anwendung. In der ersten Iterationen erstellen wir den Kontakt-Manager auf einfachste Art und Weise. Wir fügen Unterstützung für grundlegende Daten Bank Vorgänge hinzu: Create, Read, Update und DELETE (CRUD).

- Iterations #2: machen Sie das Aussehen der Anwendung schön. In dieser Iterationen verbessern wir die Darstellung der Anwendung durch Ändern der standardmäßigen ASP.NET-MVC-Ansichts Master Seite und des Cascading Stylesheets.

- Iterations #3: Formular Validierung hinzufügen. In der dritten Iterationen fügen wir die grundlegende Formular Validierung hinzu. Wir hindern Personen daran, ein Formular zu senden, ohne die erforderlichen Formularfelder abzuschließen. Wir überprüfen auch die e-Mail-Adressen und Telefonnummern.

- Iterations #4: Legen Sie die Anwendung lose gekoppelt. In dieser vierten Iterationen nutzen wir mehrere Software Entwurfsmuster, um die Verwaltung und Änderung der Contact Manager-Anwendung zu vereinfachen. Beispielsweise können wir unsere Anwendung so umgestalten, dass Sie das Repository-Muster und das Muster für die Abhängigkeitsinjektion verwendet.

- Iterations #5: Erstellen von Komponententests. In der fünften Iterationen wird die Wartung und Änderung unserer Anwendung durch Hinzufügen von Komponententests vereinfacht. Wir simulieren unsere Datenmodell Klassen und erstellen Komponententests für unsere Controller und Validierungs Logik.

- Iterations #6: Verwenden Sie die Test gesteuerte Entwicklung. In dieser sechsten Iterationen fügen wir der Anwendung neue Funktionen hinzu, indem wir zuerst Komponententests schreiben und Code für die Komponententests schreiben. In dieser Iterationen fügen wir Kontaktgruppen hinzu.

- Iterations #7: Hinzufügen von AJAX-Funktionen. In der siebten Iterationen verbessern wir die Reaktionsfähigkeit und Leistung unserer Anwendung durch Hinzufügen von Unterstützung für AJAX.

## <a name="this-iteration"></a>Diese Iterations

In dieser Iterationen der Kontakt-Manager-Anwendung wird die Anwendung für die Verwendung von AJAX umgestalten. Durch die Nutzung von AJAX machen wir unsere Anwendung reaktionsfähiger. Wir können vermeiden, dass eine ganze Seite gerendert wird, wenn nur eine bestimmte Region auf einer Seite aktualisiert werden muss.

Wir gestalten unsere Index Ansicht so um, dass wir die gesamte Seite immer dann erneut anzeigen müssen, wenn jemand eine neue Kontaktgruppe auswählt. Wenn ein Benutzer auf eine Kontaktgruppe klickt, wird stattdessen nur die Liste der Kontakte aktualisiert, und der Rest der Seite wird unverändert gelassen.

Außerdem wird die Funktionsweise des Lösch Links geändert. Anstatt eine separate Bestätigungsseite anzuzeigen, wird ein JavaScript-Bestätigungs Dialogfeld angezeigt. Wenn Sie bestätigen, dass Sie einen Kontakt löschen möchten, wird ein HTTP-Löschvorgang für den Server ausgeführt, um den Kontaktdaten Satz aus der Datenbank zu löschen.

Außerdem nutzen wir jQuery, um der Index Ansicht Animationseffekte hinzuzufügen. Wir zeigen eine Animation an, wenn die neue Liste der Kontakte vom Server abgerufen wird.

Zum Schluss nutzen wir die ASP.NET AJAX Framework-Unterstützung für die Verwaltung des Browserverlaufs. Wir erstellen Verlaufs Punkte, wenn wir einen AJAX-Aufruf zum Aktualisieren der Kontaktliste ausführen. Auf diese Weise funktionieren die Schaltflächen rückwärts und vorwärts des Browsers.

## <a name="why-use-ajax"></a>Gründe für die Verwendung von AJAX

Die Verwendung von AJAX bietet zahlreiche Vorteile. Das Hinzufügen von AJAX-Funktionen zu einer Anwendung führt zu einer besseren Benutzer Funktionalität. In einer normalen Webanwendung muss die gesamte Seite jedes Mal, wenn ein Benutzer eine Aktion ausführt, an den Server zurückgesendet werden. Wenn Sie eine Aktion ausführen, sperrt der Browser, und der Benutzer muss warten, bis die gesamte Seite abgerufen und erneut angezeigt wird.

Dies wäre bei einer Desktop Anwendung ein unzulässiger Eindruck. Aber bisher haben wir bei einer Webanwendung mit dieser ungültigen Benutzerfunktion vertraut, da wir nicht wissen, dass wir eine bessere Leistung erzielen konnten. Wir dachten, es handelte sich um eine Einschränkung von Webanwendungen, die in Wirklichkeit nur eine Einschränkung unserer imagationen war.

In einer AJAX-Anwendung müssen Sie die Benutzer Arbeit nicht einfach anhalten, um eine Seite zu aktualisieren. Stattdessen können Sie eine asynchrone Anforderung im Hintergrund ausführen, um die Seite zu aktualisieren. Sie erzwingen, dass der Benutzer nicht warten muss, während der Teil der Seite aktualisiert wird.

Wenn Sie AJAX nutzen, können Sie auch die Leistung Ihrer Anwendung verbessern. Beachten Sie, wie die Kontakt-Manager-Anwendung sofort und ohne AJAX-Funktionalität funktioniert. Wenn Sie auf eine Kontaktgruppe klicken, muss die gesamte Index Ansicht erneut angezeigt werden. Die Liste der Kontakte und die Liste der Kontaktgruppen müssen vom Datenbankserver abgerufen werden. Alle diese Daten müssen über das Netzwerk vom Webserver an den Webbrowser übermittelt werden.

Nach dem Hinzufügen von AJAX-Funktionen zu unserer Anwendung können wir jedoch vermeiden, dass die gesamte Seite erneut angezeigt wird, wenn ein Benutzer auf eine Kontaktgruppe klickt. Wir müssen die Kontaktgruppen nicht mehr aus der Datenbank erfassen. Wir müssen die gesamte Index Ansicht auch nicht über das Netzwerk übertragen. Durch die Nutzung von AJAX verringern wir die Menge an Arbeit, die der Datenbankserver ausführen muss, und wir reduzieren die Menge an Netzwerk Datenverkehr, der von der Anwendung benötigt wird.

## <a name="don-t-be-afraid-of-ajax"></a>Don 't Angst vor AJAX

Einige Entwickler vermeiden die Verwendung von AJAX, da Sie sich Gedanken über Downlevelbrowser machen. Sie möchten sicherstellen, dass Ihre Webanwendungen weiterhin funktionieren, wenn Sie von einem Browser aufgerufen werden, der JavaScript nicht unterstützt. Da AJAX von JavaScript abhängig ist, vermeiden einige Entwickler die Verwendung von AJAX.

Wenn Sie jedoch vorsichtig sind, wie Sie AJAX implementieren, können Sie Anwendungen erstellen, die sowohl mit komplexer Darstellung-als auch mit Downlevelbrowsern funktionieren. Unsere Contact Manager-Anwendung funktioniert mit Browsern, die JavaScript und Browser unterstützen, die dies nicht tun.

Wenn Sie die Kontakt-Manager-Anwendung mit einem Browser verwenden, der JavaScript unterstützt, erhalten Sie eine bessere Benutzer Leistung. Wenn Sie z. b. auf eine Kontaktgruppe klicken, wird nur der Bereich der Seite aktualisiert, in dem Kontakte angezeigt werden.

Wenn Sie auf der anderen Seite die Kontakt-Manager-Anwendung mit einem Browser verwenden, der JavaScript nicht unterstützt (oder wenn JavaScript deaktiviert ist), haben Sie eine etwas weniger wünschenswert für Benutzer. Wenn Sie z. b. auf eine Kontaktgruppe klicken, muss die gesamte Index Ansicht an den Browser zurückgesendet werden, um die passende Liste von Kontakten anzuzeigen.

## <a name="adding-the-required-javascript-files"></a>Hinzufügen der erforderlichen JavaScript-Dateien

Wir müssen drei JavaScript-Dateien verwenden, um der Anwendung AJAX-Funktionalität hinzuzufügen. Alle drei Dateien sind im Ordner Scripts einer neuen ASP.NET MVC-Anwendung enthalten.

Wenn Sie AJAX auf mehreren Seiten in Ihrer Anwendung verwenden möchten, ist es sinnvoll, die erforderlichen JavaScript-Dateien in die Ansichts Master Seite der Anwendung einzubeziehen. Auf diese Weise werden die JavaScript-Dateien automatisch in alle Seiten in der Anwendung eingeschlossen.

Fügen Sie die folgenden JavaScript-Includes innerhalb des &lt;Head&gt;-Tags der Ansichts Master Seite hinzu:

[!code-html[Main](iteration-7-add-ajax-functionality-cs/samples/sample1.html)]

## <a name="refactoring-the-index-view-to-use-ajax"></a>Umgestaltung der Index Ansicht zur Verwendung von AJAX

Beginnen Sie, indem Sie die Index Ansicht ändern, sodass beim Klicken auf eine Kontaktgruppe nur der Bereich der Ansicht aktualisiert wird, in dem Kontakte angezeigt werden. Das rote Feld in Abbildung 1 enthält den Bereich, den wir aktualisieren möchten.

[![nur aktualisieren von Kontakten](iteration-7-add-ajax-functionality-cs/_static/image1.jpg)](iteration-7-add-ajax-functionality-cs/_static/image1.png)

**Abbildung 01**: nur Kontakte werden aktualisiert ([Klicken Sie, um das Bild in voller Größe anzuzeigen](iteration-7-add-ajax-functionality-cs/_static/image2.png))

Der erste Schritt besteht darin, den Teil der Sicht, der asynchron aktualisiert werden soll, in eine separate partielle (Benutzer Steuerelement anzeigen) zu trennen. Der Abschnitt der Index Sicht, in dem die Tabelle mit Kontakten angezeigt wird, wurde in der Liste 1 in den Teil verschoben.

**Auflisten 1-views\contact\contactlist.ascx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample2.aspx)]

Beachten Sie, dass die partielle in der Auflistung 1 ein anderes Modell als die Index Ansicht aufweist. Das *erbt* -Attribut in der &lt;% @ Page%&gt;-Direktive gibt an, dass die partielle von der&lt;Group&gt; Klasse von ViewUserControl erbt.

Die aktualisierte Index Sicht ist in der Liste 2 enthalten.

**Codebeispiel 2: views\contact\index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample3.aspx)]

Es gibt zwei Dinge, die Sie über die aktualisierte Ansicht in der Liste 2 bemerken sollten. Beachten Sie zunächst, dass der gesamte Inhalt, der in die partielle verschoben wird, durch einen HTML. renderpartial ()-Code ersetzt wird. Die HTML. renderpartial ()-Methode wird aufgerufen, wenn die Index Ansicht zum ersten Mal angefordert wird, um die anfängliche Gruppe von Kontakten anzuzeigen.

Beachten Sie, dass die HTML. Action Link (), die zum Anzeigen von Kontaktgruppen verwendet wurde, durch AJAX. Action Link () ersetzt wurde. "Ajax. Action Link ()" wird mit den folgenden Parametern aufgerufen:

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample4.aspx)]

Der erste Parameter stellt den Text dar, der für den Link angezeigt werden soll, der zweite Parameter stellt die Routen Werte dar, und der dritte Parameter stellt die AJAX-Optionen dar. In diesem Fall wird die AJAX-Option updatetargetid verwendet, um auf das HTML-&lt;div&gt; Tag zu verweisen, das Sie nach Abschluss der AJAX-Anforderung aktualisieren möchten. Wir möchten das &lt;div&gt;-Tag mit der neuen Kontaktliste aktualisieren.

Die aktualisierte Index ()-Methode des Contact-Controllers ist in der Liste 3 enthalten.

**Codebeispiel 3-controllers\contactcontroller.cs (Index-Methode)**

[!code-csharp[Main](iteration-7-add-ajax-functionality-cs/samples/sample5.cs)]

Die aktualisierte Index ()-Aktion gibt bedingt einen von zwei Dingen zurück. Wenn die Index ()-Aktion durch eine AJAX-Anforderung aufgerufen wird, gibt der Controller eine partielle zurück. Andernfalls gibt die Index ()-Aktion eine gesamte Ansicht zurück.

Beachten Sie, dass die Index ()-Aktion nicht so viele Daten zurückgeben muss, wenn Sie durch eine AJAX-Anforderung aufgerufen wird. Im Kontext einer normalen Anforderung gibt die Index Aktion eine Liste aller Kontaktgruppen und der ausgewählten Kontaktgruppe zurück. Im Kontext einer AJAX-Anforderung gibt die Index ()-Aktion nur die ausgewählte Gruppe zurück. AJAX bedeutet weniger Arbeit auf dem Datenbankserver.

Unsere geänderte Index Sicht funktioniert im Fall von komplexer Darstellung-und Downlevelbrowsern. Wenn Sie auf eine Kontaktgruppe klicken und der Browser JavaScript unterstützt, wird nur der Bereich der Ansicht aktualisiert, der die Liste der Kontakte enthält. Wenn hingegen der Browser JavaScript nicht unterstützt, wird die gesamte Ansicht aktualisiert.

Die aktualisierte Index Sicht weist ein Problem auf. Wenn Sie auf eine Kontaktgruppe klicken, wird die ausgewählte Gruppe nicht hervorgehoben. Da die Liste der Gruppen außerhalb der Region angezeigt wird, die während einer AJAX-Anforderung aktualisiert wird, wird die Rechte Gruppe nicht hervorgehoben. Wir beheben dieses Problem im nächsten Abschnitt.

## <a name="adding-jquery-animation-effects"></a>Hinzufügen von jQuery-Animationseffekten

Wenn Sie auf einen Link auf einer Webseite klicken, können Sie normalerweise in der Statusleiste des Browsers erkennen, ob der Browser den aktualisierten Inhalt aktiv abrufen soll. Wenn Sie eine AJAX-Anforderung ausführen, zeigt die Statusleiste des Browsers keinen Fortschritt an. Dadurch können Benutzer nervös werden. Woher wissen Sie, ob der Browser eingefroren ist?

Es gibt mehrere Möglichkeiten, wie Sie einem Benutzer zeigen können, dass die Arbeit beim Ausführen einer AJAX-Anforderung ausgeführt wird. Ein Ansatz besteht darin, eine einfache Animation anzuzeigen. Beispielsweise können Sie eine Region ausblenden, wenn eine AJAX-Anforderung beginnt und in der Region ausgeblendet wird, wenn die Anforderung abgeschlossen ist.

Wir verwenden die jQuery-Bibliothek, die im Microsoft ASP.NET MVC-Framework enthalten ist, um die Animationseffekte zu erstellen. Die aktualisierte Index Sicht ist in der Liste 4 enthalten.

**Codebeispiel 4: views\contact\index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample6.aspx)]

Beachten Sie, dass die aktualisierte Index Sicht drei neue JavaScript-Funktionen enthält. Die ersten beiden Funktionen verwenden jQuery, um die Liste der Kontakte auszublenden und zu blenden, wenn Sie auf eine neue Kontaktgruppe klicken. Die dritte Funktion zeigt eine Fehlermeldung an, wenn eine AJAX-Anforderung zu einem Fehler führt (z. b. Netzwerk Timeout).

Die erste Funktion übernimmt auch das Hervorheben der ausgewählten Gruppe. Dem übergeordneten Element (dem Li-Element) des Elements, auf das geklickt wurde, wird ein Class = Selected-Attribut hinzugefügt. Mit jQuery können Sie das richtige Element einfach auswählen und die CSS-Klasse hinzufügen.

Diese Skripts sind mit der Hilfe des AJAX. Action Link () ajaxoptions-Parameters an die Gruppen Verknüpfungen gebunden. Der aktualisierte AJAX. Action Link ()-Methoden aufrufsieht wie folgt aus:

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample7.aspx)]

## <a name="adding-browser-history-support"></a>Hinzufügen von Browser Verlaufs Unterstützung

Wenn Sie auf einen Link klicken, um eine Seite zu aktualisieren, wird normalerweise der Browserverlauf aktualisiert. Auf diese Weise können Sie auf die Schaltfläche Zurück klicken, um zum vorherigen Zustand der Seite zurückzukehren. Wenn Sie z. b. auf die Kontaktgruppe "Freunde" klicken und dann auf die Gruppe "Geschäftskontakt" klicken, können Sie auf die Schaltfläche "zurück" klicken, um zurück zum Zustand der Seite zu navigieren, wenn die Gruppe "Freunde" ausgewählt wurde.

Leider wird beim Ausführen einer AJAX-Anforderung der Browserverlauf nicht automatisch aktualisiert. Wenn Sie auf eine Kontaktgruppe klicken und die Liste der übereinstimmenden Kontakte mit einer AJAX-Anforderung abgerufen wird, wird der Browserverlauf nicht aktualisiert. Nachdem Sie eine neue Kontaktgruppe ausgewählt haben, können Sie die Schaltfläche "Browser" nicht mehr verwenden, um zu einer Kontaktgruppe zurückzukehren.

Wenn Sie möchten, dass Benutzer nach dem Ausführen von AJAX-Anforderungen die zurück-Schaltfläche des Browsers verwenden können, müssen Sie etwas mehr Arbeit erledigen. Sie müssen die Funktion zur Verwaltung von Browser Verläufen nutzen, die im ASP.NET AJAX-Framework erstellt wurde.

ASP.NET AJAX-Browserverlauf müssen Sie drei Schritte ausführen:

1. Aktivieren Sie den Browser Verlauf, indem Sie die enablebrowserhistory-Eigenschaft auf "true" festlegen.
2. Speichern von Verlaufs Punkten, wenn sich der Zustand einer Sicht ändert, indem die AddHistoryPoint ()-Methode aufgerufen wird.
3. Rekonstruiert den Status der Ansicht, wenn das Navigate-Ereignis ausgelöst wird.

Die aktualisierte Index Sicht ist in der Liste 5 enthalten.

**Auflisten 5-views\contact\index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample8.aspx)]

In der Liste 5 ist der Browser Verlauf in der pagumit ()-Funktion aktiviert. Die pageInit ()-Funktion wird auch verwendet, um den Ereignishandler für das Navigate-Ereignis einzurichten. Das Navigate-Ereignis wird ausgelöst, wenn die Schaltfläche Browser vorwärts oder zurück bewirkt, dass der Status der Seite geändert wird.

Die begincontactlist ()-Methode wird aufgerufen, wenn Sie auf eine Kontaktgruppe klicken. Diese Methode erstellt einen neuen Verlaufs Punkt durch Aufrufen der AddHistoryPoint ()-Methode. Die ID der angeklickten Kontaktgruppe wird dem Verlauf hinzugefügt.

Die Gruppen-ID wird von einem Expando-Attribut im Link "Kontaktgruppe" abgerufen. Der Link wird mit dem folgenden-Befehl an AJAX. Action Link () gerendert.

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample9.aspx)]

Der letzte Parameter, der an AJAX. Action Link () übergeben wird, fügt dem Link ein Expando-Attribut mit dem Namen GroupID hinzu (in Kleinbuchstaben für die XHTML-Kompatibilität).

Wenn ein Benutzer auf die Schaltfläche zurück oder weiter des Browsers klickt, wird das Navigate-Ereignis ausgelöst, und die Navigate ()-Methode wird aufgerufen. Diese Methode aktualisiert die auf der Seite angezeigten Kontakte so, dass Sie mit dem Zustand der Seite übereinstimmen, die dem an die Navigate-Methode übergebenen Browserverlaufs Punkt entspricht.

## <a name="performing-ajax-deletes"></a>AJAX-Löschungen

Zum Löschen eines Kontakts müssen Sie zurzeit auf den Link löschen klicken und dann auf die Schaltfläche Löschen klicken, die auf der Seite DELETE-Bestätigung angezeigt wird (siehe Abbildung 2). Dies scheint eine große Anzahl von Seiten Anforderungen, um etwas einfaches zu tun, wie das Löschen eines Datenbankdaten Satzes.

[Seite "Löschen bestätigen" ![](iteration-7-add-ajax-functionality-cs/_static/image2.jpg)](iteration-7-add-ajax-functionality-cs/_static/image3.png)

**Abbildung 02**: die Seite zum Löschen der Bestätigung ([Klicken Sie, um das Bild in voller Größe anzuzeigen](iteration-7-add-ajax-functionality-cs/_static/image4.png))

Es ist verlockend, die Bestätigungsseite löschen zu überspringen und einen Kontakt direkt aus der Index Ansicht zu löschen. Sie sollten diese Versuchung vermeiden, da diese Vorgehensweise die Anwendung in Sicherheitslücken öffnet. Im Allgemeinen möchten Sie einen HTTP Get-Vorgang nicht ausführen, wenn Sie eine Aktion aufrufen, die den Zustand Ihrer Webanwendung ändert. Wenn Sie einen Löschvorgang ausführen, möchten Sie einen HTTP-Post-Vorgang oder einen besseren http-Löschvorgang ausführen.

Der Link "Löschen" ist Teil der Liste "contactlist". Eine aktualisierte Version von contactlist ist in der Liste 6 enthalten.

**Auflisten 6-views\contact\contactlist.ascx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample10.aspx)]

Der Link "Löschen" wird mit dem folgenden aufzurufenden Befehl der AJAX. imageaction Link ()-Methode gerendert:

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample11.aspx)]

> [!NOTE] 
> 
> AJAX. imageaction Link () ist kein Standard Teil des ASP.NET MVC-Frameworks. AJAX. imageaction Link () ist eine benutzerdefinierte Hilfsmethode, die im Contact Manager-Projekt enthalten ist.

Der Parameter "ajaxoptions" verfügt über zwei Eigenschaften. Zuerst wird die Confirm-Eigenschaft verwendet, um ein JavaScript-Bestätigungs Dialogfeld für das Popup Fenster anzuzeigen. Zweitens wird die HttpMethod-Eigenschaft verwendet, um einen HTTP DELETE-Vorgang auszuführen.

In der Liste 7 ist eine neue "ajaxdelete ()"-Aktion enthalten, die dem Contact-Controller hinzugefügt wurde.

**Codebeispiel 7-controllers\contactcontroller.cs (ajaxdelete)**

[!code-csharp[Main](iteration-7-add-ajax-functionality-cs/samples/sample12.cs)]

Die "ajaxdelete ()"-Aktion wird mit einem "Accept tverbs"-Attribut versehen. Dieses Attribut verhindert, dass die Aktion aufgerufen wird, außer durch einen anderen HTTP-Vorgang als einen HTTP-Löschvorgang. Insbesondere können Sie diese Aktion nicht mit einem HTTP Get-Vorgang aufrufen.

Nachdem Sie den Daten Bank Datensatz gelöscht haben, müssen Sie die aktualisierte Liste der Kontakte anzeigen, die nicht den gelöschten Datensatz enthält. Die ajaxdelete ()-Methode gibt die partielle contactlist-Auflistung und die aktualisierte Liste der Kontakte zurück.

## <a name="summary"></a>Zusammenfassung

In dieser Iterationen haben wir unserer Contact Manager-Anwendung AJAX-Funktionen hinzugefügt. Wir haben AJAX verwendet, um die Reaktionsfähigkeit und die Leistung unserer Anwendung zu verbessern.

Zuerst haben wir die Index Sicht umgestaltet, sodass durch Klicken auf eine Kontaktgruppe nicht die gesamte Ansicht aktualisiert wird. Wenn Sie auf eine Kontaktgruppe klicken, wird stattdessen nur die Liste der Kontakte aktualisiert.

Als nächstes haben wir jQuery-Animationseffekte verwendet, um die Liste der Kontakte abzublenden und zu blenden. Das Hinzufügen von Animationen zu einer AJAX-Anwendung kann verwendet werden, um Benutzern der Anwendung das Äquivalent zu einer Browser Statusanzeige zu bieten.

Wir haben auch die Unterstützung für den Browserverlauf unserer AJAX-Anwendung hinzugefügt Wir haben Benutzern ermöglicht, auf die Schaltflächen "zurück" und "Vorwärts" zu klicken, um den Status der Index Ansicht zu ändern.

Schließlich haben wir einen Lösch Link erstellt, der http-Löschvorgänge unterstützt. Durch das Ausführen von AJAX-Lösch Vorgängen ermöglichen wir Benutzern das Löschen von Datenbankdaten Sätzen, ohne dass der Benutzer eine zusätzliche Lösch Bestätigungsseite anfordern muss.

> [!div class="step-by-step"]
> [Zurück](iteration-6-use-test-driven-development-cs.md)
> [Weiter](iteration-1-create-the-application-vb.md)
