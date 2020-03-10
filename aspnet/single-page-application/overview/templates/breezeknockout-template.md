---
uid: single-page-application/overview/templates/breezeknockout-template
title: Breeze/Knockout-Vorlage | Microsoft-Dokumentation
author: madskristensen
description: Vorlage für Single-Page-Anwendungen für Breeze/Knockout
ms.author: riande
ms.date: 01/30/2013
ms.assetid: 3bd94827-3c59-448f-abc3-36e6df4858db
msc.legacyurl: /single-page-application/overview/templates/breezeknockout-template
msc.type: authoredcontent
ms.openlocfilehash: 5bb9ee8f758a25afa6baf3ccbaf7d5864754c7df
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449745"
---
# <a name="breezeknockout-template"></a>Breeze-/Knockout-Vorlage

von [Mads Kristensen](https://github.com/madskristensen)

> Die MVC-Vorlage "Breeze/Knockout" wurde von der Stations Glocke geschrieben.
> 
> [Herunterladen der MVC-Vorlage für Breeze/Knockout](https://go.microsoft.com/fwlink/?LinkId=282649)

Sie haben "Single Page Application" (Spa) gehört und sich gefragt, worum es sich dabei handelt. Sie können es auch für sich selbst lesen. Aber wer hat Zeit zum Herunterladen eines Beispiels? Wenn Sie Visual Studio haben, verfügen Sie über eine Beispiel-Spa, das in weniger als 60 Sekunden mit der Vorlage ASP.NET MVC 4 "Breeze/Knockout Single Page Application" ausgeführt wird.

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrRunning.png)

## <a name="what-is-the-breezeknockout-spa-template"></a>Was ist die Spa-Vorlage für Breeze/Knockout?

Die meisten Projektvorlagen generieren ein Anwendungs Skelett. Wenn Sie Ihren Code hinzufügen und schließlich eine funktionierende Anwendung bereitstellen, fügen Sie diese Gedanken auf diese Knochen. Die Spa-Vorlage für Breeze/Knockout ist unterschiedlich. Es generiert eine Beispielanwendung, die Sie untersuchen können. Es veranschaulicht ein Spa-Anwendungsdesign und viele der Techniken zum Erstellen einer Spa.

Die Vorlage "Breeze/Knockout" ist eine Variation der in der ASP.net and Web Tools 2012,2-Aktualisierung enthaltenen [knockoutjs-Spa-Vorlage](../introduction/knockoutjs-template.md) . Die Vorlage Breeze Spa generiert eine Anwendung mit der gleichen Benutzerfunktion, verfügt aber über eine andere Implementierung, die Breeze für die Datenverwaltung verwendet.

Mit der Spa-Vorlage "knockoutjs" werden Dienst Anforderungen mit dem unformatierten jQuery-AJAX erstellt, was für eine einfache Anwendung ausreichend ist. Anspruchsvollere apps haben jedoch anspruchsvollere Anforderungen an die Datenverwaltung. Beispielsweise die meisten Anwendungen:

- Abfragen und erneutes Abfragen des Servers während einer erweiterten Benutzersitzung.
- Hinzufügen von Abfrage filtern, Sortieren und Paging.
- Verwenden Sie die gleichen Daten auf mehreren Bildschirmen.
- Sammeln Sie Änderungen an vielen Objekten, und speichern Sie Sie dann als einzelne Transaktion.
- Überprüfen Sie die Änderungen auf dem Client, sodass der Benutzerfehler beheben kann, bevor die Änderungen an die Datenbank übergeben werden.

Die Bibliothek "breezejs" erledigt diese Aufgaben für Sie und bietet Ihnen die Möglichkeit, die Anwendungslogik und die benutzerfreundliche Arbeit zu entwickeln.

[**Breeze**](http://www.breezejs.com/?utm_source=ms-spa) ist eine Open-Source-Bibliothek zum Entwickeln von Rich-Data-Anwendungen in JavaScript und HTML, die apps, die in der Vergangenheit als eigenständige Desktop Anwendungen ausgeliefert wurden.

Die Vorlage "Breeze/Knockout" hilft Ihnen dabei, den ersten entscheidenden Schritt in Richtung einer stabileren Daten Verwaltungsinfrastruktur auszuführen. Sie erzeugt eine TODO-Beispielanwendung, die nach außen mit der Spa-Vorlage "knockoutjs" identisch ist. Im Inneren wird die AJAX-Datenschicht durch Breeze ersetzt, sodass Sie die beiden Ansätze nebeneinander vergleichen können. Natürlich ist das Potenzial einer Breeze-Anwendung kaum zu berühren. Sie werden jedoch sehen, wie Breeze funktioniert und wie wenig erforderlich ist, um diesen Übergang vorzunehmen.

Fangen wir an.

## <a name="create-a-breezeknockout-template-project"></a>Erstellen eines Breeze-/Knockout-Vorlagen Projekts

Um die Vorlage herunterzuladen und zu installieren, klicken Sie oben auf die Schaltfläche herunterladen Die Vorlage ist als Visual Studio-Erweiterungs Datei (VSIX) verpackt. Möglicherweise müssen Sie Visual Studio neu starten.

Wählen Sie im Bereich **Vorlagen** die Option **installierte Vorlagen** aus, und erweitern Sie den Knoten **visuelle C#**  Knoten. Wählen Sie unter **Visualisierung C#** die Option **Web**aus. Wählen Sie in der Liste der Projektvorlagen die Option **ASP.NET MVC 4-Webanwendung**aus. Geben Sie dem Projekt einen Namen, und klicken Sie auf **OK**.

Wählen Sie im Assistenten für **neue Projekte** **Breeze Knockout Spa**aus.

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeKOSpaTemplate.png)

Drücken Sie STRG + F5, um die Anwendung zu erstellen und ohne Debuggen auszuführen, oder drücken Sie F5, um das Debugging auszuführen.

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrRunning.png)

Wenn die Anwendung zum ersten Mal ausgeführt wird, wird ein Anmeldebildschirm angezeigt. Klicken Sie auf den Link "registrieren", und in der Ansicht wird eine neue Seite angezeigt, in der Sie einen Benutzernamen und ein Kennwort eingeben können. (Die Anmelde-und Registrierungsseiten werden mithilfe von ASP.NET MVC erstellt.) Wenn Sie das Registrierungsformular übermitteln, generiert der Server eine ToDoList mit zwei Elementen für Ihr Konto. Anschließend werden Sie Ihnen in einer gelben Notiz angezeigt.

![](http://www.breezejs.com/sites/all/images/spa-template/TodoList.png)

Sie befinden sich nun im Land der Spa. Alles, was Sie während der Bearbeitung von TODO sehen und haben, wird auf dem Client mithilfe von Knockout und Breeze gerendert und verwaltet. Untersuchen Sie die APP als Benutzer... aber mit der Perspektive eines Entwicklers. Verwenden Sie die Entwicklertools in Ihrem Browser, um den Netzwerk Datenverkehr zu erfassen. (In Internet Explorer: Drücken Sie F12, wählen Sie die Registerkarte **Netzwerk** aus, und klicken Sie auf **Aufzeichnung starten**.) Versuchen Sie jetzt Folgendes:

- Fügen Sie ein neues TODO-Element hinzu.
- Klicken Sie auf die Bezeichnung, und bearbeiten Sie den TODO-Element Titel
- Aktivieren Sie das Kontrollkästchen, um das Element als abgeschlossen zu markieren. Beachten Sie, dass das Textfeld deaktiviert ist, sodass der Titel nicht mehr bearbeitet werden kann.
- Klicken Sie auf das x rechts neben der Bezeichnung. Das Element wird ausgeblendet und aus der Datenbank gelöscht.
- Wählen Sie ein anderes Element aus, und löschen Sie seinen Titel Sie erhalten einen Validierungs Fehler, dass der Titel erforderlich ist. Nach einer kurzen Pause wird der vorherige Titel wieder hergestellt.
- Geben Sie einen unglaublich langen Titel ein. Sie erhalten einen anderen Validierungs Fehler, wenn der Titel zu lang ist.
- Klicken Sie auf die Schaltfläche "ToDo-Liste hinzufügen". Links neben der vorherigen Liste wird eine neue Liste angezeigt.
- Spielen Sie mit dem Titel der Liste "", und lösen Sie die erforderlichen und Längen Prüfungen aus.
- Klicken Sie in das Textfeld Titel, um die Fehlermeldung zu löschen.
- Klicken Sie im Kreis in der oberen rechten Ecke auf das "x", um die Liste und die zugehörigen Aufgaben zu löschen.

Die Validierungs Logik wird Client seitig durch Breeze ausgeführt. Validierungs Attribute für die Server Modellklassen werden an den Client weitergegeben und automatisch ausgeführt, bevor der Client den Server kontaktiert.

Überprüfen Sie den Netzwerkverkehr. Beachten Sie, dass es keine Aufrufe an den Server gab, wenn Breeze einen Fehler festgestellt hat. Jede gültige Änderung führte zu einer POST-Anforderung an "/API/TODO/SaveChanges". Breeze bündelt die Änderungen und sendet sie als einzelne Anforderung an die `SaveChanges` Methode des Web-API-Controllers. Dies unterscheidet sich von der knockoutjs-Spa-Vorlage, mit der für jedes Element einzeln Put-, Post-und DELETE-Anforderungen erstellt werden.

## <a name="peek-inside"></a>Einblick in

Diese Anwendung verfügt über eine Clientseite und eine Serverseite. Der Client seitige Stapel besteht aus einem kleinen HTML-Code und einer Kombination aus JavaScript-Anwendungsmodulen (im Ordner "App") plus Drittanbieter-JavaScript-Bibliotheken (im Ordner "Scripts").

![](http://www.breezejs.com/sites/all/images/spa-template/ClientArchitecture.png)

Wenn Sie die Vorlage "knockoutjs Spa" untersucht haben, sollte dies sehr vertraut aussehen. Konzentrieren Sie sich auf die blauen Felder. Die UI-Architektur ist Model-View-ViewModel (MVVM), bei der die HTML-Widgets der Ansicht ordnungsgemäß von dem unterstützenden Präsentationscode im Ansichts Modell getrennt sind. Ein Daten Bindungssystem (in diesem Fall Knockout) koordiniert die Ansicht und das Ansichts Modell so, dass jeder seine Aufgabe ausführen kann, ohne dass er sich mit dem anderen vertraut machen muss.

Das Modell kapselt die TODO-Daten. Entitäten im Modell werden von Breeze mit Knockout-Observable-Eigenschaften erstellt, sodass Sie direkt an Widgets in der Ansicht gebunden werden können. Das Ansichts Modell fragt den Datenkontext ab, um die Modell Entitäten abzurufen und zu speichern. Der Datenkontext delegiert den größten Teil der Arbeit an Breeze.

Der serverseitige Stapel besteht aus einem Entwickler Code und drei Grundprinzipien von .NET-Bibliotheken: Web-API, Entity Framework und Breeze.net:

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

Die grundlegende Architektur ist identisch mit der Spa-Vorlage von knockoutjs. Die Implementierung ist jedoch viel einfacher: die DTOs wurden gelöscht, und die meisten Entity Framework Details wurden an Breeze.net delegiert.

## <a name="next-steps"></a>Nächste Schritte

Wir empfehlen Ihnen, den Code zu untersuchen, der durch die [umfassende Erörterung](http://www.breezejs.com/spa-template?utm_source=ms-spa) von Client-und Server Stapeln auf der Breeze-Website gesteuert wird.

Sie können versuchen, mit der Client seitigen Breeze-Abfrage zu spielen. Fügen Sie einige Filter und Sortierungen hinzu. Sie können weitere Modell Eigenschaften und mehr Entitäten hinzufügen, um ein besseres Gefühl für die End-to-End-Entwicklung von Spa zu erhalten. Wenn Sie sicher sind, dass Sie über das Design verfügen, können Sie die TODO-Features herausreißen und durch ihre eigenen ersetzen.

Bald sind Sie bereit für den nächsten großen Schritt: das Hinzufügen von Client seitigen Bildschirmen und das Navigieren zwischen Ihnen. Sie lassen diese Spa-Vorlage unverändert und schalten einen ausführlicheren Spa-Stapel ein, z. b. das [Hot-Handtuch von John Papa](https://github.com/johnpapa/HotTowel#readme "Heißes Handtuch"), das Durandal der Breeze-und Knockout-Mischung hinzufügt.
