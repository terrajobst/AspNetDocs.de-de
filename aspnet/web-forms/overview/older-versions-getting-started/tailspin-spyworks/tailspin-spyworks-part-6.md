---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
title: 'Teil 6: ASP.NET Mitgliedschaft | Microsoft-Dokumentation'
author: JoeStagner
description: In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der Beispielanwendung Tailspin SpyWorks ausgeführt werden. Teil 6 fügt die ASP.NET-Mitgliedschaft hinzu.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: f70a310c-9557-4743-82cb-655265676d39
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
msc.type: authoredcontent
ms.openlocfilehash: b0caa89dc9ffb5bb7451fa2d9d346c7db2bf1466
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78454881"
---
# <a name="part-6-aspnet-membership"></a>Teil 6: ASP.NET Mitgliedschaft

von [Joe Stagner](https://github.com/JoeStagner)

> Tailspin SpyWorks veranschaulicht, wie einfach es ist, leistungsstarke, skalierbare Anwendungen für die .NET-Plattform zu erstellen. Es zeigt, wie die großartigen neuen Features in ASP.NET 4 verwendet werden, um einen Online Shop zu erstellen, einschließlich Einkaufs-, Checkout-und Verwaltungsfunktionen.
> 
> In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der Beispielanwendung Tailspin SpyWorks ausgeführt werden. Teil 6 fügt die ASP.NET-Mitgliedschaft hinzu.

## <a id="_Toc260221672"></a>Arbeiten mit der ASP.NET-Mitgliedschaft

![](tailspin-spyworks-part-6/_static/image1.png)

Klicken Sie auf Sicherheit

![](tailspin-spyworks-part-6/_static/image1.jpg)

Stellen Sie sicher, dass die Formular Authentifizierung verwendet wird.

![](tailspin-spyworks-part-6/_static/image2.jpg)

Verwenden Sie den Link "Benutzer erstellen", um einige Benutzer zu erstellen.

![](tailspin-spyworks-part-6/_static/image3.jpg)

Wenn Sie den Vorgang abgeschlossen haben, lesen Sie den Projektmappen-Explorer Fenster, und aktualisieren Sie die Ansicht.

![](tailspin-spyworks-part-6/_static/image2.png)

Beachten Sie, dass aspnetdb ist. Es wurde eine feine MDF-Erstellung erstellt. Diese Datei enthält die Tabellen, die die ASP.net-Kerndienste wie die Mitgliedschaft unterstützen.

Nun können wir mit der Implementierung des Checkout Prozesses beginnen.

Erstellen Sie zunächst eine Seite "Checkout. aspx".

Die Seite "Checkout. aspx" sollte nur für angemeldete Benutzer verfügbar sein, sodass der Zugriff auf angemeldete Benutzer eingeschränkt wird und Benutzer, die nicht auf der Anmeldeseite angemeldet sind, umgeleitet werden.

Zu diesem Zweck fügen wir dem Konfigurations Abschnitt der Datei "Web. config" Folgendes hinzu:

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample1.xml)]

Die Vorlage für ASP.net Web Forms-Anwendungen hat der Datei "Web. config" automatisch einen Authentifizierungs Abschnitt hinzugefügt und die Standard Anmeldeseite eingerichtet.

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample2.xml)]

Wir müssen die Code Behind-Datei "Login. aspx" so ändern, dass ein anonymer Warenkorb migriert wird, wenn sich der Benutzer anmeldet. Ändern Sie die Seite\_Lade Ereignis wie folgt.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample3.cs)]

Fügen Sie dann einen LoggedIn-Ereignishandler wie diesen hinzu, um den Sitzungs Namen auf den neu angemeldeten Benutzer festzulegen, und ändern Sie die temporäre Sitzungs-ID im Warenkorb in die des Benutzers, indem Sie die MigrateCart-Methode in der myshoppingcart-Klasse aufrufen. (In der CS-Datei implementiert)

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample4.cs)]

Implementieren Sie die MigrateCart ()-Methode wie folgt.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample5.cs)]

In "Checkout. aspx" verwenden wir eine EntityDataSource und eine GridView auf unserer Seite "Auschecken", wie auf unserer Warenkorb-Seite.

[!code-aspx[Main](tailspin-spyworks-part-6/samples/sample6.aspx)]

Beachten Sie, dass unser GridView-Steuerelement einen "ondat."-Ereignishandler mit dem Namen "myList"\_rowdatungebundene angibt. wir implementieren diesen Ereignishandler wie folgt.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample7.cs)]

Diese Methode behält einen laufenden Gesamtbetrag des Warenkorb bei, wenn jede Zeile gebunden ist, und aktualisiert die untere Zeile der GridView.

In dieser Phase haben wir eine "Review"-Präsentation der zu platzierenden Reihenfolge implementiert.

Wir behandeln ein leeres Wagen Szenario, indem wir der Seite\_Lade Ereignisses einige Codezeilen hinzufügen:

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample8.cs)]

Wenn der Benutzer auf die Schaltfläche "Senden" klickt, wird der folgende Code im Click-Ereignishandler der Schaltfläche "Senden" ausgeführt.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample9.cs)]

Das "Fleisch" des Auftrags Übermittlungs Vorgangs muss in der SubmitOrder ()-Methode der myshoppingcart-Klasse implementiert werden.

SubmitOrder führt Folgendes aus:

- Nehmen Sie alle Zeilen Elemente im Warenkorb, und verwenden Sie Sie, um einen neuen Bestelldaten Satz und die zugehörigen Order Details-Einträge zu erstellen.
- Versanddatum berechnen.
- Löschen Sie den Warenkorb.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample10.cs)]

Im Rahmen dieser Beispielanwendung berechnen wir ein Lieferdatum, indem wir einfach zwei Tage zum aktuellen Datum hinzufügen.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample11.cs)]

Wenn Sie die Anwendung jetzt ausführen, können wir den Einkaufsprozess von Anfang bis Ende testen.

> [!div class="step-by-step"]
> [Zurück](tailspin-spyworks-part-5.md)
> [Weiter](tailspin-spyworks-part-7.md)
