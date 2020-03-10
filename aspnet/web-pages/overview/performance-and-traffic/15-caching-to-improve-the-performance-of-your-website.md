---
uid: web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
title: Zwischenspeichern von Daten auf einer ASP.net Web Pages-Website (Razor) für eine bessere Leistung | Microsoft-Dokumentation
author: Rick-Anderson
description: 'Sie können Ihre Website beschleunigen, indem Sie Sie speichern, also den Cache: die Ergebnisse der Daten, die normalerweise sehr lange dauern, bis ein abgerufen oder verarbeitet wird...'
ms.author: riande
ms.date: 02/14/2014
ms.assetid: 961e525b-7700-469e-8a68-d7010b6fb68c
msc.legacyurl: /web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
msc.type: authoredcontent
ms.openlocfilehash: 01796d3ca699a6af5d9162b22a926551435c2040
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78521157"
---
# <a name="caching-data-in-an-aspnet-web-pages-razor-site-for-better-performance"></a>Zwischenspeichern von Daten auf einer ASP.net Web Pages-Website (Razor) für eine bessere Leistung

von [Tom fitzmacken](https://github.com/tfitzmac)

> In diesem Artikel wird erläutert, wie Sie mithilfe eines Hilfsprogramms Informationen für eine schnellere Leistung auf einer ASP.net Web Pages-Website (Razor) zwischenspeichern. Sie können Ihre Website beschleunigen, indem Sie Sie speichern &#8212; , indem Sie &#8212; die Ergebnisse von Daten zwischenspeichern, die normalerweise viel Zeit zum Abrufen oder verarbeiten benötigen und sich nicht häufig ändern.
> 
> **Lernen Sie Folgendes:** 
> 
> - Verwenden der Zwischenspeicherung zum Verbessern der Reaktionsfähigkeit Ihrer Website.
> 
> Dies sind die ASP.NET-Funktionen, die im Artikel eingeführt wurden:
> 
> - Das `WebCache`-Hilfsprogramm.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Im Tutorial verwendete Software Versionen
> 
> 
> - ASP.net Web Pages (Razor) 3
>   
> 
> Dieses Tutorial funktioniert auch mit ASP.net Web Pages 2.

Jedes Mal, wenn jemand eine Seite von Ihrer Website anfordert, muss der Webserver einige Schritte ausführen, um die Anforderung zu erfüllen. Für einige Ihrer Seiten muss der Server möglicherweise Aufgaben ausführen, die eine (vergleichsweise) lange Zeit in Anspruch nehmen, z. b. das Abrufen von Daten aus einer Datenbank. Auch wenn diese Aufgaben in absoluten Begriffen nicht lange dauern, kann eine ganze Reihe einzelner Anforderungen, die bewirken, dass der Webserver die komplizierte oder langsame Aufgabe durchführt, sehr viel Arbeit hinzufügen. Dies kann letztendlich die Leistung der Site beeinträchtigen.

Eine Möglichkeit, die Leistung Ihrer Website zu verbessern, ist das Zwischenspeichern von Daten. Wenn Ihre Website wiederholte Anforderungen für die gleichen Informationen erhält und die Informationen nicht für jede Person geändert werden müssen, und Sie ist nicht Zeit empfindlich, können Sie die Daten nur einmal abrufen und dann speichern, wenn Sie nicht erneut abgerufen oder neu berechnet werden. Wenn Sie das nächste Mal eine Anforderung für diese Informationen eingibt, erhalten Sie Sie einfach aus dem Cache.

Im Allgemeinen speichern Sie Informationen, die sich nicht häufig ändern. Wenn Sie Informationen in den Cache einfügen, werden Sie im Arbeitsspeicher auf dem Webserver gespeichert. Sie können angeben, wie lange der Cache zwischengespeichert werden soll (von Sekunden bis Tage). Wenn der Cache Zeitraum abläuft, werden die Informationen automatisch aus dem Cache entfernt.

> [!NOTE]
> Einträge im Cache können aus anderen Gründen entfernt werden, als Sie abgelaufen sind. Beispielsweise kann für den Webserver vorübergehend wenig Arbeitsspeicher verfügbar sein, und eine Möglichkeit zum Freigeben von Arbeitsspeicher ist das Auslösen von Einträgen aus dem Cache. Wie Sie sehen werden, müssen Sie, selbst wenn Sie Informationen in den Cache eingefügt haben, sicherstellen, dass Sie immer noch vorhanden sind, wenn Sie Sie benötigen.

Stellen Sie sich vor, Ihre Website verfügt über eine Seite mit der aktuellen Temperatur-und Wettervorhersage. Um diese Art von Informationen zu erhalten, können Sie eine Anforderung an einen externen Dienst senden. Da diese Informationen nicht sehr stark geändert werden (z. b. innerhalb eines Zeitraums von zwei Stunden) und externe Anrufe Zeit und Bandbreite erfordern, ist dies ein guter Kandidat für die Zwischenspeicherung.

## <a name="adding-caching-to-a-page"></a>Hinzufügen von Caching zu einer Seite

ASP.NET enthält eine `WebCache`-Hilfsprogramm, die das Hinzufügen von Caching zu Ihrer Website und das Hinzufügen von Daten zum Cache vereinfacht. In diesem Verfahren erstellen Sie eine Seite, auf der die aktuelle Zeit zwischengespeichert wird. Dabei handelt es sich nicht um ein reales Beispiel, da die aktuelle Zeit häufig geändert wird und das berechnen nicht kompliziert ist. Es ist jedoch eine gute Möglichkeit, das Caching in Aktion zu veranschaulichen.

1. Fügen Sie der Website eine neue Seite mit dem Namen *Webcache. cshtml* hinzu.
2. Fügen Sie den folgenden Code und das Markup der Seite hinzu:

    [!code-cshtml[Main](15-caching-to-improve-the-performance-of-your-website/samples/sample1.cshtml)]

    Wenn Sie Daten zwischenspeichern, platzieren Sie Sie im Cache mithilfe eines Namens, der auf der Website eindeutig ist. In diesem Fall verwenden Sie einen Cache Eintrag mit dem Namen `CachedTime`. Dies ist der `cacheItemKey`, der im Codebeispiel gezeigt wird.

    Der Code liest zuerst den `CachedTime` Cache Eintrag. Wenn ein Wert zurückgegeben wird (d. h., wenn der Cache Eintrag nicht NULL ist), legt der Code einfach den Wert der Zeitvariablen auf die Cache Daten fest.

    Wenn der Cache Eintrag jedoch nicht vorhanden ist (d. h., er ist NULL), legt der Code den Uhrzeitwert fest, fügt ihn dem Cache hinzu und legt einen Ablauf Wert auf eine Minute fest. Nach einer Minute wird der Cache Eintrag verworfen. (Der Standard Ablauf Wert für ein Element im Cache beträgt 20 Minuten.) Der Befehl `WebCache.Set(cacheItemKey, time, 1, false)` zeigt, wie der aktuelle Uhrzeitwert dem Cache hinzugefügt und der Ablauf auf 1 Minute festgelegt wird. Wenn der *slidingExpiration* -Parameter auf `false` festgelegt wird, bedeutet dies, dass die Ablaufzeit nicht bei jeder Anforderung erneuert wird. Sie läuft genau 1 Minute nach dem ursprünglichen hinzufügen zum Cache ab. Wenn Sie diesen Wert auf `true` festlegen, wird die Ablaufzeit von 1 Minute jedes Mal zurückgesetzt, wenn der Wert aus dem Cache angefordert wird.

    Dieser Code veranschaulicht das Muster, das Sie immer verwenden sollten, wenn Sie Daten zwischenspeichern. Bevor Sie etwas aus dem Cache heraus erhalten, überprüfen Sie immer zuerst, ob die `WebCache.Get`-Methode NULL zurückgegeben hat. Beachten Sie, dass der Cache Eintrag möglicherweise abgelaufen ist oder aus einem anderen Grund entfernt wurde, sodass ein Eintrag niemals im Cache vorhanden ist.
3. Führen Sie *Webcache. cshtml* in einem Browser aus. (Stellen Sie sicher, dass die Seite im Arbeitsbereich " **Dateien** " ausgewählt ist, bevor Sie Sie ausführen.) Wenn Sie die Seite zum ersten Mal anfordern, befinden sich die Zeit Daten nicht im Cache, und der Code muss den Zeitwert dem Cache hinzufügen.

    ![cache-1](15-caching-to-improve-the-performance-of-your-website/_static/image1.jpg)
4. Aktualisieren Sie *Webcache. cshtml* im Browser. Dieses Mal befinden sich die Zeit Daten im Cache. Beachten Sie, dass sich die Zeit seit der letzten Anzeige der Seite nicht geändert hat.

    ![cache-2](15-caching-to-improve-the-performance-of-your-website/_static/image2.jpg)
5. Warten Sie eine Minute, bis der Cache geleert wurde, und aktualisieren Sie dann die Seite. Die Seite gibt erneut an, dass die Zeit Daten nicht im Cache gefunden wurden, und die aktualisierte Zeit wird dem Cache hinzugefügt.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Anzeigen von Daten in einem Diagramm](https://go.microsoft.com/fwlink/?LinkId=202895)
- [Webcache-API-Referenz](https://msdn.microsoft.com/library/system.web.helpers.webcache(v=vs.99).aspx) (MSDN)
