---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-debugging-capabilities
title: Grundlegendes zu ASP.NET AJAX-Debugfunktionen | Microsoft-Dokumentation
author: scottcate
description: Die Möglichkeit zum Debuggen von Code ist eine Möglichkeit, die jeder Entwickler unabhängig von der verwendeten Technologie in seinem Arsenal haben sollte. Viele Entwickler sind...
ms.author: riande
ms.date: 03/28/2008
ms.assetid: 7f9380c6-19f7-4c82-a019-916ec6dffc9c
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-debugging-capabilities
msc.type: authoredcontent
ms.openlocfilehash: 08ced380f3551407d757524dbc84b5feeeb5482b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78510861"
---
# <a name="understanding-aspnet-ajax-debugging-capabilities"></a>Grundlegendes zu Debuggingfunktionen von ASP.NET AJAX

von [Scott Cate](https://github.com/scottcate)

[PDF herunterladen](https://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial06_Debugging_MS_Ajax_Applications_cs.pdf)

> Die Möglichkeit zum Debuggen von Code ist eine Möglichkeit, die jeder Entwickler unabhängig von der verwendeten Technologie in seinem Arsenal haben sollte. Obwohl viele Entwickler daran gewöhnt sind, Visual Studio .net oder Web Developer Express zum Debuggen von ASP.NET-Anwendungen C# zu verwenden, die VB.net oder Code verwenden, sind einige nicht davon abhängig, dass Sie auch für das Debuggen von Client seitigem Code wie JavaScript sehr nützlich sind. Die gleichen Techniken, die zum Debuggen von .NET-Anwendungen verwendet werden, können auch auf AJAX-aktivierte Anwendungen angewendet werden, genauer gesagt ASP.NET AJAX-Anwendungen.

## <a name="debugging-aspnet-ajax-applications"></a>Debuggen ASP.NET AJAX-Anwendungen

Dan Wahlin

Die Möglichkeit zum Debuggen von Code ist eine Möglichkeit, die jeder Entwickler unabhängig von der verwendeten Technologie in seinem Arsenal haben sollte. Es ist nicht zu sagen, dass das Verständnis der verschiedenen verfügbaren Debugoptionen einen enormen Zeitaufwand für ein Projekt und möglicherweise sogar einige Probleme einsparen kann. Obwohl viele Entwickler daran gewöhnt sind, Visual Studio .net oder Web Developer Express zum Debuggen von ASP.NET-Anwendungen C# zu verwenden, die VB.net oder Code verwenden, sind einige nicht davon abhängig, dass Sie auch für das Debuggen von Client seitigem Code wie JavaScript sehr nützlich sind. Die gleichen Techniken, die zum Debuggen von .NET-Anwendungen verwendet werden, können auch auf AJAX-aktivierte Anwendungen angewendet werden, genauer gesagt ASP.NET AJAX-Anwendungen.

In diesem Artikel erfahren Sie, wie Visual Studio 2008 und verschiedene andere Tools zum Debuggen von ASP.NET-AJAX-Anwendungen verwendet werden können, um schnell Fehler und andere Probleme zu finden. Diese Erörterung umfasst Informationen zum Aktivieren von Internet Explorer 6 oder höher für das Debuggen, die Verwendung von Visual Studio 2008 und des Skript-Explorers, um den Code schrittweise zu durchlaufen und andere kostenlose Tools wie Webentwicklungs Hilfe zu verwenden. Außerdem erfahren Sie, wie Sie ASP.NET AJAX-Anwendungen in Firefox mithilfe einer Erweiterung namens Firebug Debuggen, mit der Sie JavaScript-Code direkt im Browser ohne andere Tools durchlaufen können. Schließlich werden Sie in Klassen in der ASP.NET AJAX-Bibliothek eingeführt, die bei verschiedenen Debuggingaufgaben helfen können, wie z. b. Ablauf Verfolgung und Anweisungen zur Code Assertion.

Vor dem Debuggen von Seiten, die in Internet Explorer angezeigt werden, gibt es einige grundlegende Schritte, die Sie durchführen müssen, um Sie für das Debugging zu aktivieren. Werfen wir einen Blick auf einige grundlegende Einrichtungs Anforderungen, die für den Einstieg ausgeführt werden müssen.

## <a name="configuring-internet-explorer-for-debugging"></a>Konfigurieren von Internet Explorer für das Debugging

Die meisten Leute sind nicht daran interessiert, JavaScript-Probleme auf einer Website zu sehen, die mit Internet Explorer angezeigt wird. Tatsächlich würde der durchschnittliche Benutzer nicht einmal wissen, was zu tun ist, wenn eine Fehlermeldung angezeigt wird. Folglich werden Debugoptionen im Browser standardmäßig deaktiviert. Es ist jedoch sehr unkompliziert, das Debugging zu aktivieren und bei der Entwicklung neuer AJAX-Anwendungen zu verwenden.

Um die debuggingfunktionalität zu aktivieren, navigieren Sie im Internet Explorer-Menü zu Extras Internet Optionen, und wählen Sie die Registerkarte Erweitert Stellen Sie im Abschnitt zum Durchsuchen sicher, dass die folgenden Elemente deaktiviert sind:

- Debuggen von Skripts deaktivieren (Internet Explorer)
- Debuggen von Skripts deaktivieren (sonstige)

Wenn Sie versuchen, eine Anwendung zu debuggen, ist es zwar nicht erforderlich, aber wahrscheinlich möchten Sie, dass JavaScript-Fehler auf der Seite sofort sichtbar und offensichtlich sind. Sie können erzwingen, dass alle Fehler in einem Meldungs Feld angezeigt werden, indem Sie das Kontrollkästchen "Benachrichtigung zu den einzelnen Skript Fehlern anzeigen" aktivieren. Diese Option eignet sich zwar hervorragend, während Sie eine Anwendung entwickeln, aber Sie kann sich schnell als lästig erweisen, wenn Sie nur andere Websites nutzen, da die Wahrscheinlichkeit, dass JavaScript-Fehler auftreten, ziemlich gut ist.

Abbildung 1 zeigt, wie das Dialogfeld "Advanced Internet Explorer" aussehen sollte, nachdem es ordnungsgemäß für das Debuggen konfiguriert wurde.

[![Konfigurieren von Internet Explorer für das Debuggen.](understanding-asp-net-ajax-debugging-capabilities/_static/image2.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image1.png)

**Abbildung 1**: Konfigurieren von Internet Explorer für das Debuggen.  ([Klicken Sie, um das Bild in voller Größe anzuzeigen](understanding-asp-net-ajax-debugging-capabilities/_static/image3.png))

Nachdem das Debuggen aktiviert wurde, wird ein neues Menü Element im Menü Ansicht mit dem Namen Skript Debugger angezeigt. Es stehen zwei Optionen zur Verfügung, einschließlich öffnen und unterbrechen bei der nächsten Anweisung. Wenn Öffnen ausgewählt ist, werden Sie aufgefordert, die Seite in Visual Studio 2008 zu Debuggen (Beachten Sie, dass Visual Web Developer Express auch zum Debuggen verwendet werden kann). Wenn Visual Studio .net derzeit ausgeführt wird, können Sie diese Instanz verwenden oder eine neue Instanz erstellen. Wenn unterbrechen bei der nächsten Anweisung ausgewählt ist, werden Sie aufgefordert, die Seite zu debuggen, wenn JavaScript-Code ausgeführt wird. Wenn JavaScript-Code im OnLoad-Ereignis der Seite ausgeführt wird, können Sie die Seite aktualisieren, um eine Debugsitzung zu initiieren. Wenn JavaScript-Code nach dem Klicken auf eine Schaltfläche ausgeführt wird, wird der Debugger sofort nach dem Klicken auf die Schaltfläche ausgeführt.

> [!NOTE]
> Wenn Sie unter Windows Vista mit aktivierter Benutzer Access Control (UAC) ausgeführt werden und Visual Studio 2008 als Administrator ausführen festgelegt ist, kann Visual Studio nicht an den Prozess angefügt werden, wenn Sie zum Anfügen aufgefordert werden. Um dieses Problem zu umgehen, starten Sie zuerst Visual Studio, und verwenden Sie diese Instanz zum Debuggen.

Der nächste Abschnitt veranschaulicht, wie eine ASP.NET-AJAX-Seite direkt in Visual Studio 2008 debuggt wird. die Verwendung der Option "Skript Debugger" von Internet Explorer ist jedoch nützlich, wenn eine Seite bereits geöffnet ist und Sie Sie besser überprüfen möchten.

## <a name="debugging-with-visual-studio-2008"></a>Debuggen mit Visual Studio 2008

Visual Studio 2008 bietet Debuggingfunktionen, die Entwickler auf der ganzen Welt zum Debuggen von .NET-Anwendungen täglich einsetzen. Mit dem integrierten Debugger können Sie den Code schrittweise durchlaufen, Objektdaten anzeigen, bestimmte Variablen überwachen, die aufrufsstapel überwachen und vieles mehr. Zusätzlich zum Debuggen von C# VB.net oder Code ist der Debugger auch für das Debuggen von ASP.NET AJAX-Anwendungen hilfreich und ermöglicht es Ihnen, den JavaScript-Codezeilen Weise zu durchlaufen. Die folgenden Details konzentrieren sich auf Techniken, die zum Debuggen von Client seitigen Skriptdateien verwendet werden können, anstatt einen Diskurs zum Gesamtprozess des Debuggens von Anwendungen mit Visual Studio 2008 bereitzustellen.

Der Debugvorgang einer Seite in Visual Studio 2008 kann auf verschiedene Arten gestartet werden. Zuerst können Sie die im vorherigen Abschnitt erwähnte Internet Explorer-Skript Debugger-Option verwenden. Dies funktioniert gut, wenn eine Seite bereits im Browser geladen ist und Sie mit dem Debuggen beginnen möchten. Alternativ dazu können Sie im Projektmappen-Explorer mit der rechten Maustaste auf eine ASPX-Seite klicken und im Menü die Option als Start Seite festlegen auswählen. Wenn Sie mit dem Debuggen von ASP.NET-Seiten vertraut sind, haben Sie dies wahrscheinlich schon einmal getan. Nachdem F5 gedrückt wurde, kann die Seite deaktivierten werden. Obwohl Sie in der Regel einen Haltepunkt an einer beliebigen Stelle in VB.net oder C# Code festlegen können, ist dies bei JavaScript nicht immer der Fall.

*Eingebettete und externe Skripts*

Der Visual Studio 2008-Debugger behandelt JavaScript, das in einer anderen Seite als externe JavaScript-Dateien eingebettet ist. Mit externen Skriptdateien können Sie die Datei öffnen und einen Haltepunkt in jeder Zeile festlegen, die Sie auswählen. Haltepunkte können festgelegt werden, indem Sie auf den grauen Leistenbereich links neben dem Code-Editor-Fenster klicken. Wenn JavaScript mithilfe des `<script>`-Tags direkt in eine Seite eingebettet wird, ist es nicht möglich, einen Haltepunkt festzulegen, indem Sie auf den grauen Leistenbereich klicken. Versuche, einen Haltepunkt in einer Zeile eines eingebetteten Skripts festzulegen, führen zu einer Warnung, die besagt, dass es sich nicht um einen gültigen Speicherort für einen Haltepunkt handelt.

Sie können dieses Problem umgehen, indem Sie den Code in eine externe JS-Datei verschieben und mithilfe des src-Attributs des &lt;Skripts&gt;-Tags darauf verweisen:

[!code-html[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample1.html)]

Was geschieht, wenn das Verschieben des Codes in eine externe Datei keine Option ist oder mehr Arbeit erfordert, als dies der Fall ist? Obwohl Sie keinen Haltepunkt mithilfe des Editors festlegen können, können Sie die Debugger-Anweisung direkt in den Code einfügen, in dem Sie das Debuggen starten möchten. Sie können auch die Klasse sys. Debug verwenden, die in der ASP.NET AJAX-Bibliothek verfügbar ist, um das Debuggen zu starten. Weitere Informationen zur Klasse "sys. Debug" finden Sie weiter unten in diesem Artikel.

Ein Beispiel für die Verwendung des `debugger`-Schlüssel Worts finden Sie in der Liste 1. In diesem Beispiel wird erzwungen, dass der Debugger unmittelbar vor dem Ausführen eines Aufrufes einer Update-Funktion unterbricht.

**Codebeispiel 1. Verwenden Sie das debuggerschlüsselwort, um den Visual Studio .NET-Debugger zu unterbrechen.**

[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample2.js)]

Sobald die Debuggeranweisung gestartet wird, werden Sie aufgefordert, die Seite mithilfe von Visual Studio .net zu Debuggen und den Code schrittweise durchlaufen zu lassen. Dabei tritt möglicherweise ein Problem mit dem Zugriff auf ASP.NET-AJAX-Bibliotheks Skriptdateien auf, die auf der Seite verwendet werden. wir sehen uns nun die Verwendung von Visual Studio an. Der Skript-Explorer von net.

## <a name="using-visual-studio-net-windows-to-debug"></a>Verwenden von Visual Studio .net-Fenstern zum Debuggen

Nachdem eine Debugsitzung gestartet wurde und Sie mit der Verwendung der Standard Taste F11 beginnen, können Sie das in Abbildung 2 gezeigte Fehler Dialogfeld sehen, es sei denn, alle auf der Seite verwendeten Skriptdateien sind geöffnet und können zum Debuggen verfügbar sein.

[![Fehler Dialogfeld, das angezeigt wird, wenn kein Quellcode für das Debugging verfügbar ist.](understanding-asp-net-ajax-debugging-capabilities/_static/image5.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image4.png)

**Abbildung 2**: Fehler Dialogfeld wird angezeigt, wenn kein Quellcode zum Debuggen verfügbar ist.  ([Klicken Sie, um das Bild in voller Größe anzuzeigen](understanding-asp-net-ajax-debugging-capabilities/_static/image6.png))

Dieses Dialogfeld wird angezeigt, weil Visual Studio .net nicht sicher ist, wie der Quellcode einiger Skripts, auf die die Seite verweist, gelangt. Obwohl dies zunächst recht frustrierend sein kann, gibt es eine einfache Lösung. Wenn Sie eine Debugsitzung gestartet und einen Haltepunkt erreichen, wechseln Sie im Visual Studio 2008-Menü zum Fenster Windows-Skript-Explorer Debuggen, oder verwenden Sie den Hotkey STRG + ALT + N.

> [!NOTE]
> Wenn das Menü Skript-Explorer nicht angezeigt wird, **Navigieren Sie zu Extras > **  > **Befehle** im Visual Studio .net-Menü **Anpassen** . Suchen Sie im Abschnitt Kategorien den Eintrag **Debuggen** , und klicken Sie darauf, um alle verfügbaren Menüeinträge anzuzeigen. Scrollen Sie in der Liste Befehle nach unten zum Skript-Explorer, und ziehen Sie es in das Menü Debuggen von Windows weiter oben. Dadurch wird der Skript-Explorer-Menüeintrag bei jedem Ausführen von Visual Studio .net verfügbar.

Der Skript-Explorer kann verwendet werden, um alle Skripts anzuzeigen, die auf einer Seite verwendet werden, und diese im Code-Editor zu öffnen. Sobald der Skript-Explorer geöffnet ist, doppelklicken Sie auf die ASPX-Seite, die gerade gedebuggt wird, um Sie im Code-Editor-Fenster zu öffnen. Führen Sie die gleiche Aktion für alle anderen Skripts aus, die im Skript-Explorer angezeigt werden. Sobald alle Skripts im Code Fenster geöffnet sind, können Sie mit F11 (und den anderen Debug-Hotkeys) den Code schrittweise durchlaufen. Abbildung 3 zeigt ein Beispiel für den Skript-Explorer. Sie listet die aktuelle Datei auf, die deentschlgt wird (Demo. aspx), sowie zwei benutzerdefinierte Skripts und zwei Skripts, die vom ASP.NET AJAX ScriptManager dynamisch in die Seite eingefügt werden.

[![der Skript-Explorer ermöglicht den einfachen Zugriff auf Skripts, die auf einer Seite verwendet werden.](understanding-asp-net-ajax-debugging-capabilities/_static/image8.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image7.png)

**Abbildung 3**. Der Skript-Explorer bietet einfachen Zugriff auf Skripts, die auf einer Seite verwendet werden.  ([Klicken Sie, um das Bild in voller Größe anzuzeigen](understanding-asp-net-ajax-debugging-capabilities/_static/image9.png))

Mehrere andere Fenster können auch verwendet werden, um nützliche Informationen bereitzustellen, während Sie Code auf einer Seite durchlaufen. Beispielsweise können Sie das Fenster "lokal" verwenden, um die Werte verschiedener Variablen anzuzeigen, die auf der Seite verwendet werden, das direkt Fenster zum Auswerten bestimmter Variablen oder Bedingungen und Anzeigen der Ausgabe. Sie können das Fenster Ausgabe auch zum Anzeigen von Ablauf Verfolgungs Anweisungen verwenden, die mithilfe der Funktion sys. Debug. Trace (die später in diesem Artikel behandelt wird) oder der Debug. Write-Funktion von Internet Explorer geschrieben wurden.

Wenn Sie den Code mit dem Debugger schrittweise durchlaufen, können Sie den Mauszeiger über Variablen im Code bewegen, um den zugewiesenen Wert anzuzeigen. Der Skript Debugger zeigt jedoch gelegentlich nichts an, wenn Sie mit der Maus auf eine bestimmte JavaScript-Variable zeigen. Um den Wert anzuzeigen, markieren Sie die Anweisung oder Variable, die Sie im Code-Editor-Fenster sehen möchten, und bewegen Sie den Mauszeiger darüber. Obwohl dieses Verfahren nicht in jeder Situation funktioniert, können Sie den Wert oft anzeigen, ohne in einem anderen Debugfenster wie dem lokal Fenster suchen zu müssen.

Ein Videotutorial, das einige der hier beschriebenen Features veranschaulicht, finden Sie unter [http://www.xmlforasp.net](http://www.xmlforasp.net).

## <a name="debugging-with-web-development-helper"></a>Debuggen mit Web Development Helper

Obwohl Visual Studio 2008 (und Visual Web Developer Express 2008) sehr leistungsfähige Debuggingtools sind, können auch zusätzliche Optionen verwendet werden, die einfacher zu verwenden sind. Eines der neuesten Tools, das veröffentlicht werden soll, ist das Webentwicklungs Hilfsprogramm. Das Microsoft-Tool Nikhil Kothari (eines der wichtigsten ASP.NET-AJAX-Architekten bei Microsoft) hat dieses ausgezeichnete Tool geschrieben, das viele verschiedene Aufgaben vom einfachen Debuggen bis hin zum Anzeigen von HTTP-Anforderungs-und Antwort Nachrichten ausführen kann. Das Webentwicklungs-Hilfsprogramm kann unter [http://projects.nikhilk.net/Projects/WebDevHelper.aspx](http://projects.nikhilk.net/Projects/WebDevHelper.aspx)heruntergeladen werden.

Das Webentwicklungs-Hilfsprogramm kann direkt innerhalb von Internet Explorer verwendet werden, sodass es bequem verwendet werden kann. Es wird gestartet, indem im Internet Explorer-Menü Extras Web Development Helper ausgewählt wird. Dadurch wird das Tool im unteren Teil des Browsers geöffnet. Dies ist sehr nützlich, da Sie den Browser nicht verlassen müssen, um mehrere Aufgaben auszuführen, z. b. die HTTP-Anforderungs-und Antwort Nachrichten Protokollierung. Abbildung 4 zeigt, wie das Webentwicklungs-Hilfsprogramm in Aktion aussieht.

[![Webentwicklungs-Hilfsprogramm](understanding-asp-net-ajax-debugging-capabilities/_static/image11.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image10.png)

**Abbildung 4**: Hilfsprogramm für die Webentwicklung ([Klicken Sie, um das Bild in voller Größe anzuzeigen](understanding-asp-net-ajax-debugging-capabilities/_static/image12.png))

Webentwicklungs Hilfe ist kein Tool, das Sie verwenden, um Codezeilen Weise wie mit Visual Studio 2008 schrittweise zu durchlaufen. Sie kann jedoch verwendet werden, um die Ablauf Verfolgungs Ausgabe anzuzeigen, Variablen in einem Skript leicht auszuwerten oder Daten innerhalb eines JSON-Objekts zu untersuchen. Es ist auch sehr nützlich für das Anzeigen von Daten, die an eine ASP.NET AJAX-Seite und einen Server übermittelt werden.

Wenn das Webentwicklungs-Hilfsprogramm in Internet Explorer geöffnet ist, muss das Skript Debugging aktiviert werden, indem Sie das Skript Debuggen von Skripts aktivieren aus dem Menü Webentwicklungs Hilfe auswählen, wie oben in Abbildung 4 gezeigt Dadurch kann das Tool Fehler abfangen, die auftreten, wenn eine Seite ausgeführt wird. Außerdem ermöglicht es den einfachen Zugriff auf die auf der Seite ausgegebenen Ablauf Verfolgungs Meldungen. Um Ablauf Verfolgungs Informationen anzuzeigen oder Skript Befehle auszuführen, um verschiedene Funktionen innerhalb einer Seite zu testen, klicken Sie im Menü Webentwicklungs-Hilfsprogramm auf Skript in Skript Konsole anzeigen. Dies ermöglicht den Zugriff auf ein Befehlsfenster und ein einfaches direkt Fenster.

*Anzeigen von Ablauf Verfolgungs Meldungen und JSON-Objektdaten*

Das direkt Fenster kann verwendet werden, um Skript Befehle auszuführen oder Skripts zu laden oder zu speichern, die zum Testen verschiedener Funktionen auf einer Seite verwendet werden. Im Befehlsfenster werden die von der angezeigten Seite geschriebenen Ablauf Verfolgungs-oder Debugmeldungen angezeigt. In der Liste 2 wird gezeigt, wie eine Ablauf Verfolgungs Meldung mithilfe der Debug. Write-Funktion von Internet Explorer geschrieben wird.

**Codebeispiel 2. Schreiben einer Client seitigen Ablauf Verfolgungs Nachricht mithilfe der Debug-Klasse.**

[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample3.js)]

Wenn die LastName-Eigenschaft den Wert Doe enthält, zeigt das Webentwicklungs-Hilfsprogramm im Befehlsfenster der Skript Konsole die Meldung "Person Name: Doe" an (vorausgesetzt, dass das Debuggen aktiviert wurde). Das Hilfsprogramm für die Webentwicklung fügt der Seite auch ein debugservice-Objekt der obersten Ebene hinzu, das zum Schreiben von Ablauf Verfolgungs Informationen oder zum Anzeigen des Inhalts von JSON-Objekten verwendet werden kann. Codebeispiel 3 zeigt ein Beispiel für die Verwendung der Ablauf Verfolgungs Funktion der debugservice-Klasse.

**Codebeispiel 3. Verwenden der debugservice-Klasse von Web Development Helper zum Schreiben einer Ablauf Verfolgungs Meldung.**

[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample4.js)]

Ein nützliches Feature der debugservice-Klasse ist, dass es funktioniert, auch wenn das Debuggen in Internet Explorer nicht aktiviert ist. dadurch ist es einfach, immer auf Ablauf Verfolgungs Daten zuzugreifen, wenn das Webentwicklungs Hilfsprogramm ausgeführt wird. Wenn das Tool nicht zum Debuggen einer Seite verwendet wird, werden die Ablauf Verfolgungs Anweisungen ignoriert, da der Aufrufen von "Window. debugservice" den Wert "false" zurückgibt.

Mit der debugservice-Klasse können auch JSON-Objektdaten im Inspektor-Fenster der Webentwicklungs Hilfe angezeigt werden. In Codebeispiel 4 wird ein einfaches JSON-Objekt mit Personendaten erstellt. Nachdem das Objekt erstellt wurde, wird ein-Rückruf an die Funktion "untersuchen" der debugservice-Klasse gesendet, damit das JSON-Objekt visuell überprüft werden kann.

**Codebeispiel 4. Verwenden der debugservice. Inspect-Funktion, um JSON-Objektdaten anzuzeigen.**

[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample5.js)]

Das Aufrufen der GetPerson ()-Funktion auf der Seite oder über das direkt Fenster führt dazu, dass das Dialogfeld Objektinspektor wie in Abbildung 5 dargestellt angezeigt wird. Eigenschaften innerhalb des Objekts können dynamisch geändert werden, indem Sie hervorgehoben werden, der im Textfeld Wert angezeigte Wert geändert und dann auf den Link Aktualisieren geklickt wird. Die Verwendung des Objekt Inspektors vereinfacht das Anzeigen von JSON-Objektdaten und das Experimentieren mit dem Anwenden verschiedener Werte auf Eigenschaften.

*Debugfehler*

Zusätzlich zum Anzeigen von Ablauf Verfolgungs Daten und JSON-Objekten kann das Webentwicklungs-Hilfsobjekt auch beim Debuggen von Fehlern auf einer Seite helfen. Wenn ein Fehler auftritt, werden Sie aufgefordert, mit der nächsten Codezeile fortzufahren oder das Skript zu Debuggen (siehe Abbildung 6). Im Dialogfeld Skript Fehler werden die gesamte-aufrufsstapel sowie die Zeilennummern angezeigt, sodass Sie leicht erkennen können, wo sich Probleme innerhalb eines Skripts befinden.

[![die Verwendung des Fensters Objektinspektor zum Anzeigen eines JSON-Objekts.](understanding-asp-net-ajax-debugging-capabilities/_static/image14.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image13.png)

**Abbildung 5**: Verwenden des Fensters Objektinspektor zum Anzeigen eines JSON-Objekts.  ([Klicken Sie, um das Bild in voller Größe anzuzeigen](understanding-asp-net-ajax-debugging-capabilities/_static/image15.png))

Wenn Sie die Option Debuggen auswählen, können Sie Skript Anweisungen direkt im direkt Fenster der Webentwicklungs Hilfe ausführen, um den Wert von Variablen anzuzeigen, JSON-Objekte zu schreiben und mehr. Wenn die gleiche Aktion, die den Fehler ausgelöst hat, erneut ausgeführt wird und Visual Studio 2008 auf dem Computer verfügbar ist, werden Sie aufgefordert, eine Debugsitzung zu starten, damit Sie den Codezeilen Weise durchlaufen können, wie im vorherigen Abschnitt erläutert.

[Skript Fehler Dialogfeld für ![des Webentwicklungs Hilfsprogramms](understanding-asp-net-ajax-debugging-capabilities/_static/image17.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image16.png)

**Abbildung 6**: Skript Fehler Dialogfeld des Webentwicklungs Hilfsprogramms ([Klicken Sie, um das Bild in voller Größe anzuzeigen](understanding-asp-net-ajax-debugging-capabilities/_static/image18.png))

*Überprüfen von Anforderungs-und Antwort Nachrichten*

Beim Debuggen von ASP.NET AJAX-Seiten ist es häufig hilfreich, Anforderungs-und Antwort Nachrichten zwischen einer Seite und einem Server anzuzeigen. Wenn Sie den Inhalt in Nachrichten anzeigen, können Sie überprüfen, ob die richtigen Daten und die Größe der Nachrichten übermittelt werden. Webentwicklungs-Hilfsobjekt bietet eine hervorragende Funktion für die HTTP-Nachrichten Protokollierung, die das Anzeigen von Daten als Rohtext oder in einem besser lesbaren Format erleichtert

Um ASP.NET AJAX-Anforderungs-und-Antwort Nachrichten anzuzeigen, muss die HTTP-Protokollierung aktiviert werden, indem HTTP-Protokollierung aktivieren im Menü Webentwicklungs Hilfe ausgewählt wird. Nach der Aktivierung können alle Nachrichten, die von der aktuellen Seite gesendet werden, in der HTTP-Protokoll Anzeige angezeigt werden, auf die Sie zugreifen können, indem Sie http Show http Logs auswählen.

Obwohl das Anzeigen des in den einzelnen Anforderungs-/Antwortnachrichten gesendeten rohtexts sicherlich nützlich ist (und eine Option in Web Development Helper), ist es oft einfacher, Nachrichten Daten in einem komplexeren Format anzuzeigen. Nachdem die HTTP-Protokollierung aktiviert und Nachrichten protokolliert wurden, können die Nachrichten Daten angezeigt werden, indem Sie im HTTP-Protokoll-Viewer auf die Meldung doppelklicken. Auf diese Weise können Sie alle Header, die einer Nachricht zugeordnet sind, sowie den eigentlichen Nachrichten Inhalt anzeigen. Abbildung 7 zeigt ein Beispiel für eine Anforderungs Nachricht und eine Antwortnachricht, die im HTTP-Protokoll-Viewer-Fenster angezeigt werden.

[![die Verwendung des HTTP-Protokoll-Viewers zum Anzeigen von Anforderungs-und Antwort Nachrichten Daten.](understanding-asp-net-ajax-debugging-capabilities/_static/image20.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image19.png)

**Abbildung 7**: Verwenden des HTTP-Protokoll-Viewers zum Anzeigen von Anforderungs-und Antwort Nachrichten Daten.  ([Klicken Sie, um das Bild in voller Größe anzuzeigen](understanding-asp-net-ajax-debugging-capabilities/_static/image21.png))

Der HTTP-Protokoll-Viewer analysiert automatisch JSON-Objekte und zeigt Sie mithilfe einer Strukturansicht an, sodass die Eigenschaften Daten des Objekts schnell und einfach angezeigt werden können. Wenn ein Update Panel in einer ASP.NET AJAX-Seite verwendet wird, teilt der Viewer jeden Teil der Nachricht in einzelne Teile auf, wie in Abbildung 8 dargestellt. Dies ist ein großartiges Feature, mit dem Sie deutlich leichter erkennen und verstehen können, was in der Nachricht im Vergleich zum Anzeigen der unformatierten Nachrichten Daten der Fall ist.

[![eine mit dem HTTP-Protokoll-Viewer angezeigte Update Panel-Antwortnachricht.](understanding-asp-net-ajax-debugging-capabilities/_static/image23.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image22.png)

**Abbildung 8**: eine mit dem HTTP-Protokoll-Viewer angezeigte UpdatePanel-Antwortnachricht.  ([Klicken Sie, um das Bild in voller Größe anzuzeigen](understanding-asp-net-ajax-debugging-capabilities/_static/image24.png))

Es gibt mehrere weitere Tools, die verwendet werden können, um Anforderungs-und Antwort Nachrichten zusätzlich zum Webentwicklungs Hilfsprogramm anzuzeigen. Eine weitere gute Option ist "fddler", die unter [http://www.fiddlertool.com](http://www.fiddlertool.com)kostenlos verfügbar ist. Obwohl "fddler" hier nicht erläutert wird, ist es auch eine gute Option, wenn Sie Nachrichten Header und-Daten gründlich überprüfen müssen.

## <a name="debugging-with-firefox-and-firebug"></a>Debuggen mit Firefox und Firebug

Obwohl Internet Explorer immer noch der am häufigsten verwendete Browser ist, sind andere Browser wie Firefox sehr beliebt und werden immer häufiger verwendet. Daher sollten Sie Ihre ASP.NET AJAX-Seiten in Firefox und Internet Explorer anzeigen und Debuggen, um sicherzustellen, dass Ihre Anwendungen ordnungsgemäß funktionieren. Obwohl Firefox nicht direkt mit Visual Studio 2008 verknüpft werden kann, weist es eine Erweiterung namens Firebug auf, die zum Debuggen von Seiten verwendet werden kann. Firebug kann kostenlos heruntergeladen werden, indem Sie zu [http://www.getfirebug.com](http://www.getfirebug.com)wechseln.

Firebug bietet eine Debuggingumgebung mit vollem Funktionsumfang, die verwendet werden kann, um Codezeilen Weise zu durchlaufen, auf alle in einer Seite verwendeten Skripts zuzugreifen, Dom-Strukturen anzuzeigen, CSS-Stile anzuzeigen und sogar Ereignisse zu verfolgen, die auf einer Seite auftreten. Nachdem Sie installiert haben, können Sie auf Firebug zugreifen, indem Sie im Menü Firefox die Option Tools Firebug Open Firebug auswählen. Wie das Web Development Helper wird Firebug direkt im Browser verwendet, obwohl es auch als eigenständige Anwendung verwendet werden kann.

Sobald Firebug ausgeführt wird, können Breakpoints für jede Zeile einer JavaScript-Datei festgelegt werden, unabhängig davon, ob das Skript in eine Seite eingebettet ist. Zum Festlegen eines Breakpoints laden Sie zuerst die entsprechende Seite, die Sie in Firefox debuggen möchten. Nachdem die Seite geladen wurde, wählen Sie in der Dropdown Liste der Firebug-Skripts das zu debuggende Skript aus. Alle von der Seite verwendeten Skripts werden angezeigt. Ein Haltepunkt wird festgelegt, indem Sie auf den grauen Leistenbereich von Firebug in der Zeile klicken, in der der Breakpoint go lauten muss, wie in Visual Studio 2008.

Nachdem ein Haltepunkt in Firebug festgelegt wurde, können Sie die Aktion ausführen, die zum Ausführen des Skripts erforderlich ist, das gedebuggten werden muss, z. b. auf eine Schaltfläche klicken oder den Browser aktualisieren, um das OnLoad-Ereignis Die Ausführung wird automatisch in der Zeile angehalten, in der der Breakpoint enthalten ist. Abbildung 9 zeigt ein Beispiel für einen Haltepunkt, der in Firebug ausgelöst wurde.

[![Behandlung von Breakpoints in Firebug.](understanding-asp-net-ajax-debugging-capabilities/_static/image26.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image25.png)

**Abbildung 9**: Behandeln von Breakpoints in Firebug  ([Klicken Sie, um das Bild in voller Größe anzuzeigen](understanding-asp-net-ajax-debugging-capabilities/_static/image27.png))

Sobald ein Haltepunkt gedrückt wird, können Sie mit den Pfeil Schaltflächen schrittweise ausführen, den Code schrittweise ausführen oder den Code schrittweise ausführen. Während Sie den Code schrittweise durchlaufen, werden im rechten Bereich des Debuggers Skript Variablen angezeigt, sodass Sie Werte anzeigen und einen Drilldown in Objekte ausführen können. Firebug enthält außerdem eine Dropdown Liste für die Liste der Aufrufe, um die Ausführungs Schritte des Skripts anzuzeigen, die zur aktuellen Zeile geführt haben, die gedebuggt wird.

Firebug enthält auch ein Konsolenfenster, das zum Testen verschiedener Skript Anweisungen, Auswerten von Variablen und Anzeigen der Ablauf Verfolgungs Ausgabe verwendet werden kann. Der Zugriff erfolgt durch Klicken auf die Konsolen Registerkarte am oberen Rand des Firebug-Fensters. Die zu deaktivier Ende Seite kann auch überprüft werden, um die DOM-Struktur und den Inhalt anzuzeigen, indem Sie auf die Registerkarte Überprüfen klicken. Wenn Sie mit der Maus auf die unterschiedlichen DOM-Elemente im Inspektor-Fenster zeigen, wird der entsprechende Teil der Seite hervorgehoben, sodass Sie leicht erkennen können, wo das Element auf der Seite verwendet wird. Attributwerte, die einem bestimmten Element zugeordnet sind, können so geändert werden, dass Sie mit dem Anwenden verschiedener breiten, Stile usw. auf ein Element experimentieren können. Dies ist ein nützliches Feature, das es Ihnen erspart, zwischen dem Quellcode-Editor und dem Firefox-Browser ständig zu wechseln, um anzuzeigen, wie sich einfache Änderungen auf eine Seite auswirken.

Abbildung 10 zeigt ein Beispiel für die Verwendung des DOM-Inspektors, um ein Textfeld mit dem Namen "txtcountry" auf der Seite zu suchen. Der Firebug Inspector kann auch verwendet werden, um CSS-Stile anzuzeigen, die auf einer Seite verwendet werden, sowie Ereignisse, die auftreten, z. b. das Nachverfolgen von Mausbewegungen, Schaltflächen Klicks und mehr.

[![mit dem DOM Inspector von Firebug.](understanding-asp-net-ajax-debugging-capabilities/_static/image29.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image28.png)

**Abbildung 10**: Verwenden des DOM-Inspektors von Firebug  ([Klicken Sie, um das Bild in voller Größe anzuzeigen](understanding-asp-net-ajax-debugging-capabilities/_static/image30.png))

Firebug bietet eine einfache Möglichkeit, eine Seite direkt in Firefox zu debuggen, sowie ein hervorragendes Tool zum untersuchen verschiedener Elemente auf der Seite.

## <a name="debugging-support-in-aspnet-ajax"></a>Debuggingunterstützung in ASP.NET AJAX

Die ASP.NET AJAX-Bibliothek enthält viele verschiedene Klassen, die verwendet werden können, um den Prozess des Hinzufügens von AJAX-Funktionen zu einer Webseite zu vereinfachen. Sie können diese Klassen verwenden, um Elemente innerhalb einer Seite zu suchen und zu bearbeiten, neue Steuerelemente hinzuzufügen, Webdienste aufzurufen und sogar Ereignisse zu behandeln. Die ASP.NET AJAX-Bibliothek enthält auch Klassen, die verwendet werden können, um den Prozess des Debuggens von Seiten zu verbessern. In diesem Abschnitt werden Sie in die sys. Debug-Klasse eingeführt und sehen, wie Sie in Anwendungen verwendet werden kann.

*Verwenden der sys. Debug-Klasse*

Die sys. Debug-Klasse (eine JavaScript-Klasse im sys-Namespace) kann verwendet werden, um verschiedene Funktionen auszuführen. dazu gehören das Schreiben von Ablauf Verfolgungs Ausgaben, das Durchführen von Code Assertionen und das Erzwingen von Code, sodass der Debugger nicht ausgeführt werden kann. Sie wird in den Debugdateien der ASP.NET AJAX-Bibliothek (installiert unter c:\Programme\Microsoft ASP. net\asp.NET 2,0 AJAX Extensions\v1.0.61025\MicrosoftAjaxLibrary\System.Web.Extensions\1.0.61025.0 standardmäßig) verwendet, um bedingte Tests auszuführen ( sogenannte Assertionen), die sicherstellen, dass die Parameter ordnungsgemäß an Funktionen übermittelt werden, dass Objekte die erwarteten Daten enthalten und Ablauf Verfolgungs Anweisungen geschrieben werden.

Die sys. Debug-Klasse stellt mehrere verschiedene Funktionen zur Verfügung, die zum Verarbeiten von Ablauf Verfolgung, Code Assertionen oder Fehlern verwendet werden können, wie in Tabelle 1 dargestellt.

**Tabelle 1. Sys. Debug-Klassen Funktionen.**

| **Funktionsname** | **Beschreibung** |
| --- | --- |
| assert(condition, message, displayCaller) | Bestätigt, dass der Condition-Parameter true ist. Wenn die zu testende Bedingung false ist, wird ein Meldungs Feld verwendet, um den Wert des Nachrichten Parameters anzuzeigen. Wenn der displayCaller-Parameter true ist, zeigt die Methode auch Informationen über den Aufrufer an. |
| clearTrace() | Löscht Anweisungen aus Ablauf Verfolgungs Vorgängen. |
| fail(message) | Bewirkt, dass das Programm die Ausführung beendet und in den Debugger unterbricht. Der Message-Parameter kann verwendet werden, um einen Grund für den Fehler anzugeben. |
| Ablauf Verfolgung (Meldung) | Schreibt den Message-Parameter in die Ablauf Verfolgungs Ausgabe. |
| traceDump (Objekt, Name) | Gibt die Daten eines Objekts in einem lesbaren Format aus. Der Name-Parameter kann verwendet werden, um eine Bezeichnung für das Ablaufverfolgungs-Dump bereitzustellen. Alle untergeordneten Objekte innerhalb des Objekts, das gekippt wird, werden standardmäßig geschrieben. |

Die Client seitige Ablauf Verfolgung kann auf die gleiche Weise wie die in ASP.net verfügbare Ablauf Verfolgungs Funktionalität verwendet werden. Dadurch können verschiedene Nachrichten problemlos angezeigt werden, ohne den Ablauf der Anwendung zu unterbrechen. In der Liste 5 wird ein Beispiel für die Verwendung der sys. Debug. Trace-Funktion zum Schreiben in das Ablauf Verfolgungs Protokoll gezeigt. Diese Funktion übernimmt einfach die Nachricht, die als Parameter geschrieben werden soll.

**Codebeispiel 5. Verwenden der sys. Debug. Trace-Funktion.**

[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample6.js)]

Wenn Sie den in der Liste 5 gezeigten Code ausführen, wird keine Ablauf Verfolgungs Ausgabe auf der Seite angezeigt. Die einzige Möglichkeit, Sie zu sehen, ist die Verwendung eines Konsolenfensters, das in Visual Studio .net, Web Development Helper oder Firebug verfügbar ist. Wenn Sie die Ausgabe der Ablauf Verfolgung auf der Seite anzeigen möchten, müssen Sie ein textarea-Tag hinzufügen und ihm eine ID von TraceConsole geben, wie im folgenden dargestellt:

[!code-html[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample7.html)]

Alle sys. Debug. Trace-Anweisungen auf der Seite werden in den Textbereich TraceConsole geschrieben.

In Fällen, in denen Sie die in einem JSON-Objekt enthaltenen Daten anzeigen möchten, können Sie die traceDump-Funktion der sys. Debug-Klasse verwenden. Diese Funktion verwendet zwei Parameter, einschließlich des Objekts, das in die Ablauf Verfolgungs Konsole gekippt werden soll, und einen Namen, der zum Identifizieren des Objekts in der Ablauf Verfolgungs Ausgabe verwendet werden kann. In der Liste 6 wird ein Beispiel für die Verwendung der traceDump-Funktion gezeigt.

**Codebeispiel 6. Verwenden der sys. Debug. traceDump-Funktion**

[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample8.js)]

Abbildung 11 zeigt die Ausgabe des Aufruf der sys. Debug. traceDump-Funktion. Beachten Sie, dass zusätzlich zum Schreiben der Daten des Person-Objekts auch die Daten des address-unter Objekts geschrieben werden.

Zusätzlich zur Ablauf Verfolgung kann die Klasse sys. Debug auch zum Ausführen von Code Assertionen verwendet werden. Assertionen werden verwendet, um zu testen, ob bestimmte Bedingungen erfüllt sind, während eine Anwendung ausgeführt wird. Die Debugversion der ASP.NET-AJAX-Bibliotheks Skripts enthält mehrere Assert-Anweisungen, um eine Vielzahl von Bedingungen zu testen.

In der Liste 7 finden Sie ein Beispiel für die Verwendung der sys. Debug. Assert-Funktion zum Testen einer Bedingung. Der Code testet, ob das Adress Objekt NULL ist, bevor ein Person-Objekt aktualisiert wird.

[![Ausgabe der sys. Debug. traceDump-Funktion.](understanding-asp-net-ajax-debugging-capabilities/_static/image32.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image31.png)

**Abbildung 11**: Ausgabe der sys. Debug. traceDump-Funktion  ([Klicken Sie, um das Bild in voller Größe anzuzeigen](understanding-asp-net-ajax-debugging-capabilities/_static/image33.png))

**Codebeispiel 7: Verwenden der Debug. Assert-Funktion.**

[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample9.js)]

Es werden drei Parameter übergeben, einschließlich der auszuwertenden Bedingung, der Meldung, die angezeigt werden soll, wenn die-Überprüfung false zurückgibt und ob Informationen über den Aufrufer angezeigt werden sollen. In Fällen, in denen eine-Übersetzung fehlschlägt, wird die Meldung sowie Aufruferinformationen angezeigt, wenn der dritte Parameter true war. Abbildung 12 zeigt ein Beispiel des Fehler Dialogfelds, das angezeigt wird, wenn die in der Liste 7 angezeigte Assertion fehlschlägt.

Die abschließende zu deckende Funktion ist sys. Debug. Fail. Wenn Sie erzwingen möchten, dass Code in einer bestimmten Zeile in einem Skript fehlschlägt, können Sie einen sys. Debug. Fail-Rückruf anstelle der Debugger-Anweisung hinzufügen, die normalerweise in JavaScript-Anwendungen verwendet wird. Die sys. Debug. Fail-Funktion akzeptiert einen einzelnen Zeichen folgen Parameter, der den Grund für den Fehler darstellt, wie im folgenden dargestellt:

[!code-css[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample10.css)]

[![eine sys. Debug. Assert-Fehlermeldung.](understanding-asp-net-ajax-debugging-capabilities/_static/image35.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image34.png)

**Abbildung 12**: eine sys. Debug. Assert-Fehlermeldung.  ([Klicken Sie, um das Bild in voller Größe anzuzeigen](understanding-asp-net-ajax-debugging-capabilities/_static/image36.png))

Wenn beim Ausführen eines Skripts eine sys. Debug. Fail-Anweisung gefunden wird, wird der Wert des Message-Parameters in der Konsole einer Debuganwendung wie Visual Studio 2008 angezeigt, und Sie werden aufgefordert, die Anwendung zu debuggen. Ein Fall, in dem dies sehr nützlich sein kann, ist, wenn Sie keinen Haltepunkt mit Visual Studio 2008 in einem Inline Skript festlegen können, der Code jedoch auf einer bestimmten Zeile angehalten werden soll, damit Sie den Wert der Variablen überprüfen können.

*Grundlegendes zur ScriptMode-Eigenschaft des ScriptManager-Steuer Elements*

Die ASP.NET AJAX-Bibliothek umfasst Debug-und Release-Skript Versionen, die standardmäßig unter c:\Programme\Microsoft ASP. net\asp.NET 2,0 AJAX Extensions\v1.0.61025\MicrosoftAjaxLibrary\System.Web.Extensions\1.0.61025.0 installiert werden. Die Debugskripts sind gut formatiert, leicht lesbar und weisen mehrere sys. Debug. Assert-Aufrufe auf, während die Releaseskripts leer sind, und verwenden die Klasse sys. Debug, um Ihre Gesamtgröße zu minimieren.

Das dem ASP.NET AJAX-Seiten hinzugefügte ScriptManager-Steuerelement liest das debug-Attribut des Kompilierungs Elements in Web. config, um zu bestimmen, welche Versionen der Bibliotheks Skripts geladen werden Sie können jedoch steuern, ob Debug-oder Releaseskripts geladen werden (Bibliotheks Skripts oder eigene benutzerdefinierte Skripts), indem Sie die ScriptMode-Eigenschaft ändern. ScriptMode akzeptiert eine ScriptMode-Enumeration, deren Member Auto, Debug, Release und Erben einschließen.

ScriptMode ist standardmäßig auf den Wert "Auto" festgelegt. Dies bedeutet, dass der ScriptManager das debug-Attribut in "Web. config" überprüft. Wenn Debug auf false fest steht, lädt ScriptManager die Releaseversion der ASP.NET AJAX-Bibliotheks Skripts. Wenn Debuggen den Wert true hat, wird die Debugversion der Skripts geladen. Wenn die ScriptMode-Eigenschaft in Release oder Debug geändert wird, wird der ScriptManager gezwungen, die entsprechenden Skripts zu laden, unabhängig davon, welchen Wert das debug-Attribut in Web. config hat. In der Liste 8 wird ein Beispiel für das Verwenden des ScriptManager-Steuer Elements zum Laden von Debugskripts aus der ASP.NET AJAX-Bibliothek

Codebeispiel **8. Laden von Debugskripts mithilfe von ScriptManager**.

[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample11.aspx)]

Sie können auch verschiedene Versionen (Debug oder Release) ihrer eigenen benutzerdefinierten Skripts laden, indem Sie die Scripts-Eigenschaft von ScriptManager zusammen mit der scriptreferenzierungskomponente verwenden, wie in der Abbildung 9 dargestellt.

**Codebeispiel 9. Laden benutzerdefinierter Skripts mithilfe von ScriptManager.**

[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample12.aspx)]

> [!NOTE]
> Wenn Sie benutzerdefinierte Skripts mithilfe der scriptreferenzierungskomponente laden, müssen Sie den ScriptManager Benachrichtigen, wenn das Skript den Ladevorgang abgeschlossen hat, indem Sie den folgenden Code am Ende des Skripts hinzufügen:

[!code-csharp[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample13.cs)]

Der in der Liste 9 angezeigte Code weist den ScriptManager an, nach einer Debugversion des Person-Skripts zu suchen, damit "Person. Debug. js" anstelle von "Person. js" automatisch gesucht wird. Wenn die Person. Debug. js-Datei nicht gefunden wird, wird ein Fehler ausgelöst.

In Fällen, in denen eine Debug-oder Releaseversion eines benutzerdefinierten Skripts basierend auf dem Wert der ScriptMode-Eigenschaft geladen werden soll, die für das ScriptManager-Steuerelement festgelegt wurde, können Sie die ScriptMode-Eigenschaft des scriptreferenzierungssteuer Elements auf erben festlegen. Dies bewirkt, dass die richtige Version des benutzerdefinierten Skripts basierend auf der ScriptMode-Eigenschaft von ScriptManager geladen wird, wie in der Liste 10 dargestellt. Da die ScriptMode-Eigenschaft des ScriptManager-Steuer Elements auf "Debug" festgelegt ist, wird das "Person. Debug. js"-Skript geladen und auf der Seite verwendet.

**Codebeispiel 10. Erbt den ScriptMode von ScriptManager für benutzerdefinierte Skripts.**

[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample14.aspx)]

Mithilfe der ScriptMode-Eigenschaft können Sie Anwendungen einfacher Debuggen und den gesamten Prozess vereinfachen. Die Releaseskripts der ASP.NET AJAX-Bibliothek sind recht schwierig zu durchlaufen und zu lesen, da die Code Formatierung entfernt wurde, während die Debugskripts speziell für Debugzwecke formatiert sind.

## <a name="conclusion"></a>Zusammenfassung

Die ASP.NET AJAX-Technologie von Microsoft bietet eine solide Grundlage zum Entwickeln von AJAX-fähigen Anwendungen, die die Gesamtleistung des Endbenutzers verbessern können. Wie bei jeder Programmier Technologie treten jedoch auch Fehler und andere Anwendungsprobleme auf. Wenn Sie die verschiedenen verfügbaren Debugoptionen kennen, können Sie viel Zeit sparen und zu einem stabileren Produkt führen.

In diesem Artikel wurden verschiedene Verfahren zum Debuggen von ASP.NET AJAX-Seiten, einschließlich Internet Explorer mit Visual Studio 2008, Web Development Helper und Firebug, vorgestellt. Diese Tools können den gesamten Debugprozess vereinfachen, da Sie auf Variablen Daten zugreifen, Codezeilen Weise durchlaufen und Ablauf Verfolgungs Anweisungen anzeigen können. Zusätzlich zu den verschiedenen behandelten Debuggingtools haben Sie auch gesehen, wie die sys. Debug-Klasse der ASP.NET AJAX-Bibliothek in einer Anwendung verwendet werden kann und wie die ScriptManager-Klasse zum Laden von Debug-oder Releaseversionen von Skripts verwendet werden kann.

## <a name="bio"></a>Basierten

Dan Wahlin (Microsoft Most Valuable Professional für ASP.net und XML Web Services) ist ein .net-Entwicklungs Dozenten und-architekturberater bei Interface Technical Training ([www.interfacett.com)](http://www.interfacett.com). Dan gründete die XML for ASP.NET Developers-Website ([www.XMLforASP.net](http://www.XMLforASP.NET)), ist im Büro des INETA-Referenten und spricht auf mehreren Konferenzen. Dan hat eine professionelle Windows-DNA (Wrox), ASP.net: Tipps, Tutorials und Code (Sams), ASP.NET 1,1 Insider Solutions, Professional ASP.NET 2,0 AJAX (Wrox), ASP.NET 2,0 MVP-Hacks und erstellte XML for ASP.NET Developers (Sams) verfasst. Beim Schreiben von Code, Artikeln oder Büchern hat Dan das Schreiben und Aufzeichnen von Musik und das Spielen von Golf und Basketball mit seiner Frau und ihren Kindern.

Scott Cate arbeitet seit 1997 mit Microsoft-Webtechnologien und ist der Präsident von myKB.com ([www.myKB.com](http://www.myKB.com)), wo er sich darauf spezialisiert hat, ASP.NET basierte Anwendungen zu schreiben, die sich auf die Software Lösungen der Wissensdatenbank konzentrieren. Scott kann über [scott.cate@myKB.com](mailto:scott.cate@myKB.com) oder seinen Blog unter [ScottCate.com](http://ScottCate.com) per e-Mail kontaktiert werden.

> [!div class="step-by-step"]
> [Previous](understanding-asp-net-ajax-web-services.md)
