---
uid: single-page-application/overview/templates/breezeangular-template
title: Breeze/Angular-Vorlage | Microsoft-Dokumentation
author: madskristensen
description: Anwendung "Breeze/Angular Single Page Application"
ms.author: riande
ms.date: 03/08/2013
ms.assetid: db31e909-563a-4516-aadd-62aa210ac7e4
msc.legacyurl: /single-page-application/overview/templates/breezeangular-template
msc.type: authoredcontent
ms.openlocfilehash: 3e4e63d385a56d51d3d08696782b43d6228f6201
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78467187"
---
# <a name="breezeangular-template"></a>Breeze-/Angular-Vorlage

von [Mads Kristensen](https://github.com/madskristensen)

> Die MVC-Vorlage "Breeze/Angular" wurde von der Stations Glocke geschrieben.
> 
> [Herunterladen der MVC-Vorlage für Breeze/Angular](https://go.microsoft.com/fwlink/?LinkId=286437)

[Angularjs](http://angularjs.org) ist eine Open-Source-Bibliothek von Google zum Entwickeln von Single-Page-Anwendungen (Spas). Sie bietet Datenbindung, Abhängigkeitsinjektion und Bildschirm Verwaltung. Kombinieren Sie es mit " [breezejs](http://www.breezejs.com/?utm_source=ms-spa)", einer anderen Open-Source-Bibliothek für die Datenmodellierung und Datenverwaltung, und Sie verfügen über die wesentlichen Komponenten für eine großartige HTML-/Javas-Client-

Die Vorlage "Breeze/Angular Spa" ist eine Variation der in der ASP.net and Web Tools 2012,2-Aktualisierung enthaltenen [knockoutjs-Spa-Vorlage](../introduction/knockoutjs-template.md) . Wenn Sie Visual Studio haben, verfügen Sie über eine Beispiel-Spa, die in weniger als 60 Sekunden ausgeführt wird.

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningTodoPage.png)

Im Außenbereich sieht die Anwendung ähnlich wie die "knockoutjs"-Spa-Vorlage aus. Es ist jedoch ganz anders. Die knockoutjs-Vorlage verwendet Knockout für die Datenbindung und den unformatierten AJAX für den Datenzugriff. Die Breeze-/Angular-Vorlage verwendet Angular für die Datenbindung und Breeze für den Datenzugriff. Diese Bibliotheken ermöglichen zusätzliche Funktionen, einschließlich Seitennavigation und-Verlauf.

Hier ist die Info-Seite der Anwendung:

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningAboutPage.png)

Auf dieser Seite wird ein Ereignisprotokoll angezeigt, das während der aktuellen Benutzersitzung ausgeführt wird, einschließlich:

- Paging. Beachten Sie die TODO-Controller Erstellung bei #2 und #7.
- Remote Abfragen (#3) und lokale Cache Abfragen (#7).
- Neue (#5, #6) und geänderte (#4) Entitäten werden gespeichert.
- Auf dem Client (#9) validierte Änderungen, sodass der Benutzerfehler beheben kann, bevor Änderungen an die Datenbank übergeben werden.

In dieser Vorlage finden Sie weitere Informationen, darunter:

- Dynamisches Laden von HTML-Ansichts Vorlagen.
- Benutzerdefinierte Datenbindung durch Angular-Direktiven.
- Modularität und Abhängigkeitsinjektion.
- Abfragen von Filtern, sortieren, Paging, Projektionen und Einbindung verwandter Entitäten.
- Freigeben von Daten auf mehreren Bildschirmen.
- Speichern mehrerer Änderungen als einzelne Transaktion.
- Validierungsregeln werden automatisch vom Server an den JavaScript-Client weitergegeben.

Fangen wir an.

## <a name="create-a-breezeangular-template-project"></a>Erstellen eines Breeze-/Angular-Vorlagen Projekts

Um die Vorlage herunterzuladen und zu installieren, klicken Sie oben auf die Schaltfläche herunterladen Die Vorlage ist als Visual Studio-Erweiterungs Datei (VSIX) verpackt. Möglicherweise müssen Sie Visual Studio neu starten.

Wählen Sie im Bereich **Vorlagen** die Option **installierte Vorlagen** aus, und erweitern Sie den Knoten **visuelle C#**  Knoten. Wählen Sie unter **Visualisierung C#** die Option **Web**aus. Wählen Sie in der Liste der Projektvorlagen die Option **ASP.NET MVC 4-Webanwendung**aus. Geben Sie dem Projekt einen Namen, und klicken Sie auf **OK**.

Wählen Sie im Assistenten für **neue Projekte** **Breeze Angular Spa**aus.

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeNgSpaTemplate.png)

Drücken Sie STRG + F5, um die Anwendung zu erstellen und ohne Debuggen auszuführen, oder drücken Sie F5, um das Debugging auszuführen.

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrLogin.png)

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
- Klicken Sie oben rechts auf den Link "Info", um ein Protokoll dieser Aktivitäten anzuzeigen.

Die Validierungs Logik wird Client seitig durch Breeze ausgeführt. Validierungs Attribute für die Server Modellklassen werden an den Client weitergegeben und automatisch ausgeführt, bevor der Client den Server kontaktiert.

Überprüfen Sie den Netzwerkverkehr. Beachten Sie, dass es keine Aufrufe an den Server gab, wenn Breeze einen Fehler festgestellt hat. Jede gültige Änderung führte zu einer POST-Anforderung an "/API/TODO/SaveChanges". Breeze bündelt die Änderungen und sendet sie als einzelne Anforderung an die `SaveChanges` Methode des Web-API-Controllers. Dies unterscheidet sich von der knockoutjs-Spa-Vorlage, mit der für jedes Element einzeln Put-, Post-und DELETE-Anforderungen erstellt werden.

Beachten Sie außerdem, dass kein Netzwerk Datenverkehr vorhanden ist, wenn Sie zwischen der Liste der Aufgaben und den Seiten wechseln. Das liegt daran, dass die Abfrage auf den lokalen Breeze-Cache beschränkt wurde.

## <a name="peek-inside"></a>Einblick in

Diese Anwendung verfügt über eine Clientseite und eine Serverseite. Der Client seitige Stapel besteht aus einem kleinen HTML-Code und einer Kombination aus JavaScript-Anwendungsmodulen (im Ordner "App") plus Drittanbieter-JavaScript-Bibliotheken (im Ordner "Scripts").

![](http://www.breezejs.com/sites/all/images/spa-template/NgClientArchitecture2.png)

Die UI-Architektur trennt die HTML-Widgets der Ansichten von dem unterstützenden Präsentationscode in den Controllern. Das Angular-Daten Bindungssystem koordiniert Sichten und Controller so, dass jede seine Aufgabe ausführen kann, ohne dass Sie sich mit dem anderen vertraut machen müssen.

Der Controller fragt den Datenkontext ab, um die Modell Entitäten abzurufen und zu speichern. Der Datenkontext delegiert den größten Teil der Arbeit an Breeze, wodurch Modell Objekte mit selbst Nachverfolgung aus JSON-Abfrage Ergebnissen erstellt werden.

Der serverseitige Stapel besteht aus einem Entwickler Code und drei Grundprinzipien von .NET-Bibliotheken: Web-API, Entity Framework und Breeze.net:

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

Die grundlegende Architektur ist identisch mit der Spa-Vorlage von knockoutjs. Die Implementierung ist jedoch viel einfacher: die DTOs wurden gelöscht, und die meisten Entity Framework Details wurden an Breeze.net delegiert.

## <a name="next-steps"></a>Nächste Schritte

Wir empfehlen Ihnen, den Code zu untersuchen, der durch die [umfassende Erörterung](http://www.breezejs.com/ng-spa-template?utm_source=ms-spa) von Client-und Server Stapeln auf der Breeze-Website gesteuert wird.

Sie können versuchen, mit der Client seitigen Breeze-Abfrage zu spielen. Fügen Sie einige Filter und Sortierungen hinzu. Sie können weitere Modell Eigenschaften und mehr Entitäten hinzufügen, um ein besseres Gefühl für die End-to-End-Entwicklung von Spa zu erhalten. Wenn Sie sicher sind, dass Sie über das Design verfügen, können Sie die TODO-Features herausreißen und durch ihre eigenen ersetzen.

Viel Spaß beim Programmieren!
