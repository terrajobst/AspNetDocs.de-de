---
uid: web-pages/overview/performance-and-traffic/14-analyzing-traffic
title: Nachverfolgen von Besucher Informationen (Analytics) für eine ASP.net Web Pages (Razor-) Site | Microsoft-Dokumentation
author: Rick-Anderson
description: Nachdem Sie Ihre Website erhalten haben, möchten Sie möglicherweise den Datenverkehr Ihrer Website analysieren.
ms.author: riande
ms.date: 02/17/2014
ms.assetid: 360bc6e1-84c5-4b8e-a84c-ea48ab807aa4
msc.legacyurl: /web-pages/overview/performance-and-traffic/14-analyzing-traffic
msc.type: authoredcontent
ms.openlocfilehash: 095a5572c755446e0661c052ca9de82d636429fd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78421455"
---
# <a name="tracking-visitor-information-analytics-for-an-aspnet-web-pages-razor-site"></a>Nachverfolgen von Besucher Informationen (Analytics) für eine ASP.net Web Pages (Razor-) Site

von [Tom fitzmacken](https://github.com/tfitzmac)

> In diesem Artikel wird beschrieben, wie Sie mit einem Hilfsprogramm Website Analysen zu Seiten auf einer ASP.net Web Pages-Website (Razor) hinzufügen.
> 
> Sie lernen Folgendes:
> 
> - Informationen zum Senden von Informationen über den Datenverkehr Ihrer Website an einen Analyse Anbieter.
> 
> Dies sind die ASP.net-Programmierfunktionen, die im Artikel eingeführt wurden:
> 
> - Das `Analytics`-Hilfsprogramm.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Im Tutorial verwendete Software Versionen
> 
> 
> - ASP.net Web Pages (Razor) 2
> - ASP.net-webhilfsprogramme-Bibliothek (nuget-Paket)

Analytics ist ein allgemeiner Begriff für Technologie, die den Datenverkehr auf Ihrer Website misst, damit Sie verstehen können, wie Benutzer die Website verwenden. Viele Analysedienste sind verfügbar, darunter Dienste von Google, Yahoo, Status Counter und anderen.

Die Vorgehensweise bei der Analyse besteht darin, dass Sie sich für ein Konto mit dem Analyse Anbieter registrieren, in dem Sie die Website registrieren, die Sie nachverfolgen möchten. Der Anbieter sendet Ihnen einen Ausschnitt von JavaScript-Code, der eine ID oder einen nach Verfolgungs Code für Ihr Konto enthält. Sie fügen den JavaScript-Code Ausschnitt den Webseiten auf der Website hinzu, die Sie nachverfolgen möchten. (Sie fügen den Analyse Ausschnitt in der Regel zu einer Fußzeile oder Layoutseite oder zu einem anderen HTML-Markup hinzu, das auf jeder Seite der Website angezeigt wird.) Wenn Benutzer eine Seite anfordern, die einen dieser JavaScript-Ausschnitte enthält, sendet der Ausschnitt Informationen über die aktuelle Seite an den Analyse Anbieter, der verschiedene Details zur Seite aufzeichnet.

Wenn Sie sich Ihre Site Statistik ansehen möchten, melden Sie sich bei der Website des Analytics-Anbieters an. Sie können dann alle Arten von Berichten zu Ihrer Website anzeigen, wie z. b.:

- Die Anzahl der Seitenaufrufe für einzelne Seiten. Dies teilt Ihnen (ungefähr), wie viele Personen die Website besuchen und welche Seiten auf Ihrer Website am beliebtesten sind.
- Gibt an, wie lange die Benutzer für bestimmte Seiten aufwenden. So können Sie erkennen, ob Ihre Startseite die Interessen der Menschen beibehält.
- Auf welchen Websites sich die Mitarbeiter befanden, bevor Sie Ihre Website besucht haben. Dies hilft Ihnen zu verstehen, ob der Datenverkehr von Verknüpfungen, von Such Vorgängen usw. stammt.
- Wenn die Benutzer Ihre Website besuchen und wie lange Sie verbringen.
- Aus welchen Ländern die Besucher stammen.
- Welche Browser und Betriebssysteme von den Besuchern verwendet werden.

    ![Ch14traffic-1](14-analyzing-traffic/_static/image1.jpg)

## <a name="using-a-helper-to-add-analytics-to-a-page"></a>Verwenden eines Hilfsobjekts zum Hinzufügen von Analysen zu einer Seite

ASP.net Web Pages umfasst mehrere Analyse Hilfen (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`und `Analytics.GetStatCounterHtml`), die die Verwaltung der für die Analyse verwendeten JavaScript-Ausschnitte erleichtern. Anstatt herauszufinden, wie und wo der JavaScript-Code abgelegt werden soll, müssen Sie nur das Hilfsprogramm zu einer Seite hinzufügen. Die einzigen Informationen, die Sie bereitstellen müssen, sind Ihr Konto Name, Ihre ID oder der nach Verfolgungs Code. (Für den Status Counter müssen Sie auch einige zusätzliche Werte angeben.)

In diesem Verfahren erstellen Sie eine Layoutseite, die das `GetGoogleHtml`-Hilfsprogramm verwendet. Wenn Sie bereits über ein Konto mit einem der anderen Analyse Anbieter verfügen, können Sie stattdessen dieses Konto verwenden und bei Bedarf geringfügige Anpassungen vornehmen.

> [!NOTE]
> Wenn Sie ein Analytics-Konto erstellen, registrieren Sie die URL der Website, die Sie nachverfolgen möchten. Wenn Sie alles auf dem lokalen Computer testen, wird der tatsächliche Datenverkehr (nur der Datenverkehr) nachverfolgt, sodass Sie keine Website Statistiken aufzeichnen und anzeigen können. Dieses Verfahren zeigt jedoch, wie Sie einer Seite ein Analytics-Hilfsprogramm hinzufügen. Wenn Sie Ihre Website veröffentlichen, sendet die Live Site Informationen an ihren Analyse Anbieter.

1. Fügen Sie die ASP.net-webhilfsbibliothek Ihrer Website hinzu, wie unter [Installieren von Hilfsprogrammen an einer ASP.net Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372)beschrieben, wenn Sie Sie noch nicht hinzugefügt haben.
2. Erstellen Sie ein Konto mit Google Analytics, und notieren Sie sich den Kontonamen.
3. Erstellen Sie eine Layoutseite mit dem Namen *Analytics. cshtml* , und fügen Sie das folgende Markup hinzu:

    [!code-cshtml[Main](14-analyzing-traffic/samples/sample1.cshtml)]

    > [!NOTE]
    > Sie müssen den aufzurufenden `Analytics`-Hilfsprogramms im Text der Webseite (vor dem `</body>`-Tag) platzieren. Andernfalls führt der Browser das Skript nicht aus.

    Wenn Sie einen anderen Analyse Anbieter verwenden, verwenden Sie stattdessen eines der folgenden Hilfsprogramme:

    - (Yahoo) `@Analytics.GetYahooHtml("myaccount")`
    - (Status Counter) `@Analytics.GetStatCounterHtml("project", "security")`
4. Ersetzen Sie `myaccount` durch den Namen des Kontos, der ID oder des nach Verfolgungs Codes, den Sie in Schritt 1 erstellt haben.
5. Führen Sie die Seite im Browser aus. (Stellen Sie sicher, dass die Seite im Arbeitsbereich " **Dateien** " ausgewählt ist, bevor Sie Sie ausführen.)
6. Zeigen Sie im Browser die Seitenquelle an. Der gerenderte Analyse Code kann angezeigt werden:

    [!code-html[Main](14-analyzing-traffic/samples/sample2.html)]
7. Melden Sie sich bei der Google Analytics-Website an, und überprüfen Sie die Statistiken für Ihre Website Wenn Sie die Seite auf einer Live Website ausführen, wird ein Eintrag angezeigt, der den Besuch auf Ihrer Seite protokolliert.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Google Analytics-Website](https://www.google.com/analytics/)
- [Yahoo! Web Analytics-Website](http://help.yahoo.com/l/us/yahoo/ywa/)
- [Status Counter-Website](http://statcounter.com/)
