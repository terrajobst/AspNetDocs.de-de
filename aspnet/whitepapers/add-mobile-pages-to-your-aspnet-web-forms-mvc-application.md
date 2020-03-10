---
uid: whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application
title: 'Vorgehensweise: Hinzufügen von mobilen Seiten zu Ihrer ASP.net-Web Forms/MVC-Anwendung | Microsoft-Dokumentation'
author: rick-anderson
description: In diesem Thema werden verschiedene Möglichkeiten beschrieben, wie Sie für mobile Geräte optimierte Seiten von Ihrer ASP.net-Web Forms/MVC-Anwendung bereitstellen, und es werden Architektur und...
ms.author: riande
ms.date: 01/20/2011
ms.assetid: 3124f28e-cc32-418a-afe3-519fa56f4c36
msc.legacyurl: /whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application
msc.type: content
ms.openlocfilehash: 63c555358d06a9506bb5c8c993800c3307108192
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78462213"
---
# <a name="how-to-add-mobile-pages-to-your-aspnet-web-forms--mvc-application"></a>Gewusst wie: Hinzufügen von mobilen Seiten zu Ihrer ASP.net-Web Forms/MVC-Anwendung

> **Gilt für**
> 
> - ASP.net Web Forms Version 4,0
> - ASP.NET MVC, Version 3,0
> 
> **Zusammenfassung**
> 
> In diesem Thema werden verschiedene Möglichkeiten zum Verarbeiten von Seiten beschrieben, die von Ihrer ASP.net Web Forms/MVC-Anwendung für mobile Geräte optimiert werden, und es werden Architektur-und Entwurfs Probleme vorgeschlagen, die für eine große Bandbreite von Geräten zu beachten sind. Außerdem wird in diesem Dokument erläutert, warum die mobilen ASP.NET-Steuerelemente von ASP.NET 2,0 bis 3,5 mittlerweile veraltet sind und einige moderne Alternativen erläutert.

## <a name="contents"></a>Inhalt

- Übersicht
- Architektur Optionen
- Browser-und Geräteerkennung
- Funktionsweise von ASP.net-Web Forms Anwendungen für Mobile spezifische Seiten
- Funktionsweise von ASP.NET MVC-Anwendungen für Mobile spezifische Seiten
- Zusätzliche Ressourcen

Herunterladbare Codebeispiele, die die Techniken dieses Whitepaper für ASP.net Web Forms und MVC veranschaulichen, finden Sie unter [Mobile Apps & Websites mit ASP.net](https://docs.microsoft.com/aspnet/mobile/overview).

## <a name="overview"></a>Übersicht

Mobile Geräte – Smartphones, featuretelefone und Tablets – werden weiterhin als Mittel für den Zugriff auf das Web beliebter. Für viele Webentwickler und weborientierte Unternehmen bedeutet dies, dass es immer wichtiger ist, den Besuchern, die diese Geräte verwenden, eine tolle Browser Darstellung zu bieten.

### <a name="how-earlier-versions-of-aspnet-supported-mobile-browsers"></a>Wie frühere Versionen von ASP.NET Mobile Browser unterstützen

ASP.NET-Versionen 2,0 bis 3,5 enthalten *ASP.NET Mobile*-Steuerelemente: einen Satz von Server Steuerelementen für mobile Geräte in der Assembly " *System. Web. Mobile. dll* " und den *System. Web. UI. MobileControls* -Namespace. Die Assembly ist weiterhin in ASP.NET 4 enthalten, ist jedoch veraltet. Entwicklern wird empfohlen, zu moderneren Ansätzen zu migrieren, wie z. b. die in diesem Whitepaper beschriebenen.

Der Grund, warum ASP.NET Mobile-Steuerelemente als veraltet markiert wurden, besteht darin, dass Ihr Entwurf auf Mobiltelefone ausgerichtet ist, die ungefähr 2005 und früher üblich waren. Die-Steuerelemente dienen hauptsächlich zum Rendering von WML-oder cHTML-Markup (anstelle von regulärem HTML) für die WAP-Browser dieses Zeitraums. WAP, WML und cHTML sind jedoch für die meisten Projekte nicht mehr relevant, da HTML nun die universelle Markup Sprache für Mobile und Desktop Browser gleichermaßen geworden ist.

### <a name="the-challenges-of-supporting-mobile-devices-today"></a>Die Herausforderungen der Unterstützung mobiler Geräte heute

Obwohl Mobile Browser nun nahezu universell HTML unterstützen, werden Sie bei der Erstellung hervorragend mobiler Browser Probleme trotzdem viele Probleme meistern:

- ***Bildschirmgröße*** : Mobile Geräte variieren in der Form erheblich, und ihre Bildschirme sind häufig viel kleiner als Desktop Monitore. Daher müssen Sie möglicherweise vollständig verschiedene Seitenlayouts für Sie entwerfen.
- ***Eingabemethoden*** – einige Geräte verfügen über Keypads, von denen einige über Styluses verfügen, von denen andere Finger Eingaben verwenden. Möglicherweise müssen Sie mehrere Navigationsmechanismen und Dateneingabe Methoden in Erwägung gezogen.
- ***Einhaltung von Standards*** – viele mobile Browser unterstützen nicht die neuesten HTML-, CSS-oder JavaScript-Standards.
- ***Bandbreite*** – die Netzwerkleistung der Mobilfunkdaten variiert stark, und einige Endbenutzer sind an Tarifen, die mit Megabyte abgerechnet werden.

Es gibt keine Lösung mit einer Größen Lösung. die Anwendung muss sich entsprechend dem Gerät, auf das zugegriffen wird, unterschiedlich ansehen und Verhalten. Je nachdem, welche Ebene von mobilem Support Sie benötigen, kann dies für Webentwickler eine größere Herausforderung darstellen als der Desktop "browserkriege".

Entwickler, die sich zum ersten Mal mit der Unterstützung für Mobile Browser beschäftigen, sind häufig der Ansicht, dass es nur wichtig ist, die neuesten und ausgereiftesten Smartphones (z. b. Windows Phone 7, iPhone oder Android) zu unterstützen, vielleicht weil Entwickler häufig eine solche Ling. Kostengünstigere Telefone sind jedoch immer noch sehr beliebt, und ihre Besitzer verwenden Sie, um das Web zu durchsuchen – besonders in Ländern, in denen Mobiltelefone leichter zu erreichen sind als eine Breitbandverbindung. Ihr Unternehmen muss entscheiden, welche Anzahl von Geräten unterstützt werden soll, indem er seine wahrscheinlichen Kunden berücksichtigt. Wenn Sie eine Online-Broschüre für eine Luxus-Health-Spa erstellen, können Sie eine geschäftliche Entscheidung treffen, dass Sie nur auf Erweiterte Smartphones abzielen. Wenn Sie ein Ticket Reservierungssystem für ein Kino erstellen, müssen Sie wahrscheinlich Besucher mit weniger leistungsfähigem Feature berücksichtigen. Ker.

## <a name="architectural-options"></a>Architektur Optionen

Beachten Sie, dass Webentwickler im Allgemeinen drei wichtige Optionen für die Unterstützung mobiler Browser haben, bevor Sie die speziellen technischen Details von ASP.net Web Forms oder MVC kennenlernen.

1. ***Do Nothing –*** Sie können einfach eine standardmäßige, Desktop orientierte Webanwendung erstellen und sich auf Mobile Browser stützen, um Sie auf akzeptable Weise zu gestalten. 

    - **Vorteil**: Dies ist die günstigste Option zur Implementierung und Wartung – ohne zusätzliche Arbeit
    - **Nachteil**: bietet das schlechteste Endbenutzer Verhalten: 

        - Die neuesten Smartphones können Ihren HTML-Code ebenso wie einen Desktop Browser Rendering, aber die Benutzer sind immer noch gezwungen, horizontal und vertikal zu zoomen und zu scrollen, um Ihre Inhalte auf einem kleinen Bildschirm zu nutzen. Dies ist weit von der optimalen Lösung.
        - Ältere Geräte und featuretelefone können das Markup möglicherweise nicht zufriedenstellend darstellen.
        - Auch auf den neuesten Tablet-Geräten (deren Bildschirme so groß wie Laptop Bildschirme sein können) sind unterschiedliche Interaktions Regeln anwendbar. Die Berührungs basierte Eingabe funktioniert am besten mit größeren Schaltflächen und Verknüpfungen, die weiter auseinander liegen, und es gibt keine Möglichkeit, mit dem Mauszeiger auf ein ausgefülltes Menü zu zeigen.
2. ***Beheben Sie das Problem auf dem Client* – durch die** sorgfältige Verwendung von CSS und [progressivem Verb esse](http://en.wikipedia.org/wiki/Progressive_enhancement) rungen können Sie Markup, Stile und Skripts erstellen, die an den Browser angepasst werden, auf dem Sie ausgeführt werden. Beispielsweise können Sie mit [CSS 3-Medien Abfragen](http://www.w3.org/TR/2010/CR-css3-mediaqueries-20100727/)ein mehrspaltige Layout erstellen, das sich in einem einzelnen Spalten Layout auf Geräten befindet, deren Bildschirme schmaler als ein ausgewählter Schwellenwert sind. 

    - **Vorteil**: optimiert das Rendering für das jeweilige verwendete Gerät, auch für unbekannte zukünftige Geräte entsprechend dem Bildschirm und den eingegebenen Merkmalen.
    - **Vorteil**: Sie können auf einfache Weise serverseitige Logik für alle Gerätetypen freigeben – minimale Duplizierung von Code oder Aufwand
    - **Nachteil**: Mobile Geräte unterscheiden sich so von Desktop Geräten, dass sich Ihre mobilen Seiten möglicherweise vollständig von den Desktop Seiten unterscheiden, wobei andere Informationen angezeigt werden. Diese Variationen können ineffizient oder unmöglich sein, allein durch CSS zu erreichen, insbesondere bei der Interpretation von CSS-Regeln durch inkonsistente ältere Geräte. Dies gilt insbesondere für CSS 3-Attribute.
    - **Nachteil**: bietet keine Unterstützung für unterschiedliche serverseitige Logik und Workflows für verschiedene Geräte. Sie können z. b. einen vereinfachten Workflow zum Auschecken von Einkaufswagen für mobile Benutzer über CSS allein implementieren.
    - **Nachteil**: ineffiziente Bandbreiten Verwendung. Der Server muss möglicherweise Markup und Stile übertragen, die auf alle möglichen Geräte angewendet werden, auch wenn das Zielgerät nur eine Teilmenge dieser Informationen verwendet.
3. ***Beheben Sie das Problem auf dem Server* –** Wenn Ihr Server weiß, auf welches Gerät er zugreift – oder zumindest die Merkmale dieses Geräts, wie z. b. Bildschirmgröße und Eingabemethode, und ob es sich um ein mobiles Gerät handelt – kann eine andere Logik ausgeführt werden, und es kann ein anderes HTML-Markup ausgegeben werden. 

    - **Vorteil:** Maximale Flexibilität. Es gibt keine Beschränkung, wie viel Sie die serverseitige Logik für mobile Geräte verändern oder das Markup für das gewünschte, gerätespezifische Layout optimieren können.
    - **Vorteil:** Effiziente Bandbreiten Verwendung. Sie müssen lediglich Markup-und Formatierungsinformationen übertragen, die vom Zielgerät verwendet werden.
    - **Nachteil:** Manchmal erzwingt die Wiederholung von Aufwand oder Code (z. b. das Erstellen ähnlicher, aber etwas unterschiedlicher Kopien Ihrer Web Forms Seiten oder MVC-Ansichten). Wenn möglich, sollten Sie gängige Logik in eine zugrunde liegende Schicht oder einen zugrunde liegenden Dienst einbeziehen, aber dennoch müssen einige Teile Ihres UI-Codes oder Markups dupliziert und parallel gewartet werden.
    - **Nachteil:** Geräteerkennung ist nicht trivial. Hierfür ist eine Liste oder Datenbank bekannter Gerätetypen und deren Merkmale (die möglicherweise nicht immer auf dem neuesten Stand sind) erforderlich, und es ist nicht garantiert, dass jede eingehende Anforderung genau entspricht. In diesem Dokument werden einige Optionen und ihre Fehler zu einem späteren Zeitpunkt beschrieben.

Um die besten Ergebnisse zu erzielen, werden die meisten Entwickler feststellen, dass Sie Optionen (2) und (3) kombinieren müssen. Kleinere stilistische Unterschiede werden am besten auf dem Client unter Verwendung von CSS oder sogar JavaScript untergebracht, während wichtige Unterschiede von Daten, Workflows und Markup am effektivsten in Server seitigem Code implementiert werden.

### <a name="this-paper-focuses-on-server-side-techniques"></a>Dieser Artikel konzentriert sich auf serverseitige Techniken.

Da ASP.net Web Forms und MVC in erster Linie serverseitige Technologien sind, konzentriert sich dieses Whitepaper auf serverseitige Techniken, mit denen Sie unterschiedliche Markup-und Logik Optionen für Mobile Browser entwickeln können. Natürlich können Sie dies auch mit beliebigen Client seitigen Techniken (z. b. CSS 3-Medien Abfragen, progressivem JavaScript) kombinieren, aber das ist eine Frage des webentwurfs, als die ASP.net-Entwicklung.

## <a name="browser-and-device-detection"></a>Browser-und Geräteerkennung

Die wichtigste Voraussetzung für alle serverseitigen Techniken zur Unterstützung mobiler Geräte ist das wissen, welches Gerät Ihr Besucher verwendet. Tatsächlich ist es sogar noch besser, den Hersteller und die Modellnummer des Geräts zu kennen und die *Merkmale* des Geräts zu kennen. Die Merkmale können Folgendes umfassen:

- Handelt es sich um ein mobiles Gerät?
- Eingabemethode (Maus/Tastatur, Toucheingabe, Tastatur, Joystick,...)
- Bildschirmgröße (physisch und in Pixel)
- Unterstützte Medien und Datenformate
- usw.

Es ist besser, Entscheidungen auf der Grundlage von Merkmalen als Modellnummer zu treffen, denn dann sind Sie besser für die Verarbeitung zukünftiger Geräte gerüstet.

### <a name="using-aspnets-built-in-browser-detection-support"></a>Mithilfe von ASP. Unterstützung der integrierten Browser Erkennung in net

ASP.net Web Forms-und MVC-Entwickler können durch Überprüfen der Eigenschaften des *Request. Browser* -Objekts wichtige Merkmale eines Browser-Browsers sofort ermitteln. Informationen finden Sie beispielsweise unter.

- Request.Browser.IsMobileDevice
- Request.Browser.MobileDeviceManufacturer, Request.Browser.MobileDeviceModel
- Request.Browser.ScreenPixelsWidth
- Request.Browser.SupportsXmlHttp
- ... und viele weitere

Im Hintergrund stimmt die ASP.NET-Plattform mit dem eingehenden *Benutzer-Agent* (UA)-HTTP-Header für reguläre Ausdrücke in einem Satz von Browser Definitions-XML-Dateien überein. Standardmäßig enthält die Plattform Definitionen für viele gängige Mobile Geräte, und Sie können benutzerdefinierte Browser Definitions Dateien für andere Benutzer hinzufügen, die Sie erkennen möchten. Weitere Informationen finden Sie auf der MSDN [-Seite ASP.NET Webserver-Steuerelemente und Browser Funktionen](https://msdn.microsoft.com/library/x3k2ssx2.aspx).

### <a name="using-the-wurfl-device-database-via-51degreesmobi-foundation"></a>Verwenden der wurfl-Geräte Datenbank über 51Degrees.mobi Foundation

Während ASP. Die integrierte Browser Erkennungs Unterstützung von NET ist für viele Anwendungen ausreichend. es gibt zwei Hauptfälle, in denen Sie möglicherweise nicht ausreicht:

- ***Sie möchten die neuesten Geräte erkennen***(ohne manuelle Erstellung von Browser Definitions Dateien). Beachten Sie, dass die Browser Definitions Dateien von .NET 4 nicht aktuell genug sind, um Windows Phone 7, Android-Telefone, Opera Mobile-Browser oder Apple iPads zu erkennen.
- ***Sie benötigen ausführlichere Informationen zu den Gerätefunktionen***. Möglicherweise müssen Sie sich über die Eingabemethode eines Geräts (z. b. touchvs Keypad) oder über die Audioformate informieren, die der Browser unterstützt. Diese Informationen sind in den standardmäßigen Browser Definitions Dateien nicht verfügbar.

Das [wurfl-Projekt ( *Wireless Universal Resource File* )](http://wurfl.sourceforge.net/) bietet noch viel mehr aktuelle und ausführliche Informationen zu den heute verwendeten mobilen Geräten.

Die großartige Nachricht für .NET-Entwickler ist das ASP. Die Browser Erkennungsfunktion von NET ist erweiterbar, sodass es möglich ist, diese Probleme zu beheben. Beispielsweise können Sie dem Projekt die Open Source- [*51Degrees.mobi Foundation*](https://github.com/51Degrees/dotNET-Device-Detection) -Bibliothek hinzufügen. Dabei handelt es sich um einen ASP.net IHttpModule-oder Browser Funktions Anbieter (sowohl in Web Forms-als auch in MVC-Anwendungen verwendbar), der die wurfl-Daten direkt liest und Sie an ASP bindet. Integrierter Browser Erkennungsmechanismus in net. Nachdem Sie das Modul installiert haben, enthält " *Request. Browser* " plötzlich viel genauere und ausführlichere Informationen: Es erkennt viele der bereits erwähnten Geräte ordnungsgemäß und listet die Funktionen auf (einschließlich zusätzlicher Funktionen wie z. b. Eingabemethode). Weitere Informationen finden Sie in der Dokumentation des Projekts.

## <a name="how-web-forms-applications-can-present-mobile-specific-pages"></a>Funktionsweise von Web Forms Anwendungen für Mobile spezifische Seiten

Standardmäßig wird hier die Art, wie eine neue Web Forms Anwendung auf gängigen mobilen Geräten angezeigt wird:

![](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/_static/image1.png)

Natürlich sieht keines der beiden Layouts sehr mobil, – diese Seite wurde für einen großen, quer orientierten Monitor entworfen, nicht für einen kleinen Hochformat orientierten Bildschirm. Was können Sie also tun?

Wie bereits weiter oben in diesem Artikel erläutert, gibt es viele Möglichkeiten, Ihre Seiten für mobile Geräte anzupassen. Einige Techniken sind Server basiert, andere werden auf dem Client ausgeführt.

### <a name="creating-a-mobile-specific-master-page"></a>Erstellen einer mobilen-spezifischen Master Seite

Abhängig von Ihren Anforderungen können Sie möglicherweise die gleichen Web Forms für alle Besucher verwenden, aber über zwei separate Masterseiten verfügen: eine für Desktop Besucher, eine für Besucher mobiler Besucher. Dadurch haben Sie die Flexibilität, das CSS-Stylesheet oder das HTML-Markup der obersten Ebene so zu ändern, dass es für mobile Geräte geeignet ist, ohne dass Sie eine beliebige Seiten Logik duplizieren müssen.

Dies ist einfach. Beispielsweise können Sie einem Webformular einen PreInit-Handler wie den folgenden hinzufügen:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample1.cs)]

Erstellen Sie jetzt eine Master Seite mit dem Namen "Mobile. Master" im Ordner der obersten Ebene Ihrer Anwendung, die verwendet wird, wenn ein mobiles Gerät erkannt wird. Ihre Mobile Master Seite kann bei Bedarf auf ein Mobile-spezifisches CSS-Stylesheet verweisen. Desktop Besucher sehen weiterhin Ihre Standard Master Seite, nicht die Mobile.

### <a name="creating-independent-mobile-specific-web-forms"></a>Erstellen unabhängiger, mobiler Web Forms

Um maximale Flexibilität zu erreichen, können Sie viel weiter gehen, als nur separate Masterseiten für verschiedene Gerätetypen zu haben. Sie können zwei *vollständig getrennte Sätze von Web Forms Seiten* implementieren – einen Satz für Desktop Browser, einen weiteren Satz für mobile Geräte. Dies funktioniert am besten, wenn Sie mobilen Besuchern sehr unterschiedliche Informationen oder Workflows präsentieren möchten. Im restlichen Teil dieses Abschnitts wird dieser Ansatz ausführlich beschrieben.

Wenn Sie bereits über eine Web Forms Anwendung verfügen, die für Desktop Browser konzipiert ist, können Sie am einfachsten fortfahren, indem Sie in Ihrem Projekt einen Unterordner mit dem Namen "Mobile" erstellen und dort Ihre mobilen Seiten erstellen. Sie können eine gesamte unter Website mit eigenen Masterseiten, Stylesheets und Seiten erstellen, indem Sie dieselben Techniken verwenden, die Sie auch für andere Web Forms-Anwendungen verwenden. Sie müssen nicht unbedingt für *jede* Seite der Desktop Site eine Mobile-Entsprechung entwickeln. Sie können auswählen, welche Teilmenge der Funktionalität für Mobile Besucher sinnvoll ist.

Wenn Sie möchten, können Ihre mobilen Seiten allgemeine statische Ressourcen (z. b. Bilder, JavaScript oder CSS-Dateien) mit ihren regulären Seiten gemeinsam verwenden. Da Ihr "mobiler" Ordner *nicht* als separate Anwendung gekennzeichnet ist, wenn er in IIS gehostet wird (es handelt sich lediglich um einen einfachen Unterordner von Web Forms Seiten), werden auch die gleiche Konfiguration, Sitzungsdaten und andere Infrastruktur wie Ihre Desktop Seiten verwendet.

> [!NOTE]
> Da dieser Ansatz in der Regel eine Duplizierung von Code beinhaltet (Mobile Seiten haben wahrscheinlich einige Ähnlichkeiten mit Desktop Seiten gemeinsam), ist es wichtig, dass Sie allgemeine Geschäftslogik oder Datenzugriffs Code in eine freigegebene zugrunde liegende Ebene oder einen gemeinsamen Dienst übertragen. Andernfalls verdoppeln Sie den Aufwand für das Erstellen und Verwalten Ihrer Anwendung.

#### <a name="redirecting-mobile-visitors-to-your-mobile-pages"></a>Umleiten mobiler Besucher an Ihre mobilen Seiten

Häufig ist es praktisch, Mobile Besucher nur bei der *ersten* Anforderung in ihrer Browsersitzung (und nicht bei jeder Anforderung in Ihrer Sitzung) an die mobilen Seiten umzuleiten, weil Folgendes der Fall ist:

- Sie können dann auf einfache Weise mobilen Besuchern den Zugriff auf Ihre Desktop Seiten gestatten – indem Sie einfach einen Link auf der Master Seite ablegen, der zu "Desktop Version" wechselt. Der Besucher wird nicht zurück an eine mobile Seite umgeleitet, da es sich nicht mehr um die erste Anforderung in der Sitzung handelt.
- Dadurch wird das Risiko vermieden, dass Anforderungen für dynamische Ressourcen beeinträchtigt werden, die von Desktop-und mobilen Teilen Ihrer Site gemeinsam genutzt werden (z. b. Wenn Sie über ein gängiges Webformular verfügen, das sowohl Desktop-als auch Mobile Teile Ihrer Website in einem IFRAME oder bestimmte AJAX-Handler anzeigt).

Zu diesem Zweck können Sie die Umleitungs Logik in einer **Sitzungs\_Start** -Methode platzieren. Fügen Sie z. b. der Global.asax.cs-Datei die folgende Methode hinzu:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample2.cs)]

#### <a name="configuring-forms-authentication-to-respect-your-mobile-pages"></a>Konfigurieren der Formular Authentifizierung zur Berücksichtigung ihrer mobilen Seiten

Beachten Sie, dass bei der Formular Authentifizierung bestimmte Annahmen darüber getroffen werden, wo Besucher während und nach dem Authentifizierungsprozess umgeleitet werden können:

- Wenn ein Benutzer authentifiziert werden muss, wird er von der Formular Authentifizierung an die Desktop Anmeldeseite umgeleitet, unabhängig davon, ob es sich um einen Desktop Benutzer oder einen mobilen Benutzer handelt (da nur ein Konzept *einer Anmelde-* URL verwendet wird). Wenn Sie Ihre Mobile Anmeldeseite auf andere Weise formatieren möchten, müssen Sie Ihre Desktop Anmeldeseite verbessern, damit Mobile Benutzer zu einer separaten mobilen Anmeldeseite umgeleitet werden. Fügen Sie beispielsweise den folgenden Code zu Ihrer **Desktop** -Anmeldeseite Code-Behind hinzu: 

    [!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample3.cs)]
- Nachdem sich ein Benutzer erfolgreich angemeldet hat, leitet die Formular Authentifizierung diese standardmäßig zur Desktop Startseite um (da *nur ein Konzept einer Standardseite* verwendet wird). Sie müssen Ihre Mobile Anmeldeseite so erweitern, dass Sie nach einer erfolgreichen Anmeldung an die mobile Homepage umgeleitet wird. Fügen Sie z. b. den folgenden Code zu Ihrer **Mobile** Login Page Code-Behind hinzu: 

    [!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample4.cs)]
  
  Dieser Code setzt voraus, dass Ihre Seite über ein Login-Server Steuerelement namens LoginUser verfügt, wie in der Standard Projektvorlage.

### <a name="working-with-output-caching"></a>Arbeiten mit Ausgabe Caching

Wenn Sie die Ausgabe Zwischenspeicherung verwenden, achten Sie darauf, dass ein Desktop Benutzer standardmäßig eine bestimmte URL aufrufen kann (wodurch die Ausgabe zwischengespeichert wird), gefolgt von einem mobilen Benutzer, der dann die zwischengespeicherte Desktop Ausgabe empfängt. Diese Warnung gilt unabhängig davon, ob Sie Ihre Master Seite nur nach Gerätetyp variieren oder vollständig getrennte Web Forms pro Gerätetyp implementieren.

Um das Problem zu vermeiden, können Sie ASP.NET anweisen, den Cache Eintrag entsprechend der Frage zu verändern, ob der Besucher ein mobiles Gerät verwendet. Fügen Sie der OutputCache-Deklaration Ihrer Seite einen VaryByCustom-Parameter wie folgt hinzu:

[!code-aspx[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample5.aspx)]

Definieren Sie als nächstes *IsMobileDevice* als benutzerdefinierten Cache Parameter, indem Sie die folgende Methoden Überschreibung zu ihrer Global.asax.cs-Datei hinzufügen:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample6.cs)]

Dadurch wird sichergestellt, dass Mobile Besucher der Seite keine Ausgabe erhalten, die zuvor von einem Desktop Besucher in den Cache eingefügt wurde.

### <a name="a-working-example"></a>Ein funktionierendes Beispiel

Um diese Techniken in Aktion zu sehen, laden Sie [die Codebeispiele dieses Whitepaper](https://docs.microsoft.com/aspnet/mobile/overview)herunter. Die Web Forms Beispielanwendung leitet Mobile Benutzer automatisch an eine Reihe von mobilen Seiten in einem Unterordner mit dem Namen "Mobile" weiter. Das Markup und die Formatierung dieser Seiten sind für Mobile Browser besser optimiert, wie in den folgenden Screenshots zu sehen ist:

![](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/_static/image2.png)

Weitere Tipps zum Optimieren von Markup und CSS für Mobile Browser finden Sie im Abschnitt "Formatieren von mobilen Seiten für Mobile Browser" weiter unten in diesem Dokument.

## <a name="how-aspnet-mvc-applications-can-present-mobile-specific-pages"></a>Funktionsweise von ASP.NET MVC-Anwendungen für Mobile spezifische Seiten

Da das Model-View-Controller-Muster die Anwendungslogik (in Controllern) von der Präsentationslogik (in Sichten) entkoppelt, können Sie eine der folgenden Ansätze für die Behandlung von mobilen Unterstützung in Server seitigem Code auswählen:

1. ***die gleichen Controller und Ansichten sowohl für Desktop-als auch für Mobile Browser verwenden, die Ansichten aber je nach Gerätetyp mit unterschiedlichen Razor-Layouts darstellen.** Diese Option funktioniert am besten, wenn Sie identische Daten auf allen Geräten anzeigen, aber einfach andere CSS-Stylesheets bereitstellen oder einige HTML-Elemente der obersten Ebene für Mobiltelefone ändern möchten.
2. ***Verwenden Sie für Desktop-und Mobile Browser dieselben Controller, aber je nach Gerätetyp unterschiedliche Ansichten***. Diese Option funktioniert am besten, wenn Sie ungefähr dieselben Daten anzeigen und die gleichen Workflows für Endbenutzer bereitstellen möchten, aber ein sehr anderes HTML-Markup für das verwendete Gerät darstellen möchten.
3. ***separate Bereiche für Desktop-und Mobile Browser erstellen, die für jede * unabhängige Controller und Ansichten implementieren.** Diese Option funktioniert am besten, wenn Sie sehr unterschiedliche Bildschirme mit unterschiedlichen Informationen anzeigen und den Benutzer durch verschiedene Workflows leiten, die für den Gerätetyp optimiert sind. Dies kann eine Wiederholung von Code bedeuten. Sie können dies jedoch minimieren, indem Sie eine gemeinsame Logik in eine zugrunde liegende Ebene oder einen zugrunde liegenden Dienst einbeziehen.

Wenn Sie die **erste** Option nutzen und nur das Razor-Layout pro Gerätetyp variieren möchten, ist es sehr einfach. Ändern Sie einfach die \_viewstart. cshtml-Datei wie folgt:

[!code-cshtml[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample7.cshtml)]

Nun können Sie ein mobiles spezifisches Layout mit dem Namen \_layoutmobile. cshtml mit einer für mobile Geräte optimierten Seitenstruktur und CSS-Regeln erstellen.

Wenn Sie die **zweite** Option zum Rendering ganz verschiedener Ansichten gemäß dem Gerätetyp des Besuchers nutzen möchten, lesen Sie den [Blogbeitrag von Scott Hanselman](http://www.hanselman.com/blog/ABetterASPNETMVCMobileDeviceCapabilitiesViewEngine.aspx).

Im restlichen Teil dieses Artikels geht es um die **dritte** Option – Erstellen separater Controller *und* Ansichten für mobile Geräte – damit Sie genau steuern können, welche Teilmenge der Funktionen für Mobile Besucher angeboten wird.

### <a name="setting-up-a-mobile-area-within-your-aspnet-mvc-application"></a>Einrichten eines mobilen Bereichs in Ihrer ASP.NET MVC-Anwendung

Sie können einen Bereich namens "Mobile" zu einer vorhandenen ASP.NET MVC-Anwendung auf normale Weise hinzufügen: Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen, und wählen Sie dann Bereich hinzufügen aus. Sie können dann Controller und Ansichten wie für jeden anderen Bereich in einer ASP.NET MVC-Anwendung hinzufügen. Fügen Sie z. b. einen neuen Controller namens HomeController zum mobilen Bereich hinzu, der als Homepage für Mobile Besucher fungiert.

### <a name="ensuring-the-url-mobile-reaches-the-mobile-homepage"></a>Sicherstellen, dass die URL/Mobile die mobile Homepage erreicht

Wenn Sie möchten, dass die URL/Mobile die Index Aktion in HomeController innerhalb Ihres mobilen Bereichs erreicht, müssen Sie zwei kleine Änderungen an der Routing Konfiguration vornehmen. Aktualisieren Sie zunächst Ihre mobilearearegistration-Klasse so, dass HomeController der Standard Controller in Ihrem mobilen Bereich ist, wie im folgenden Code gezeigt:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample8.cs)]

Dies bedeutet, dass sich die mobile Homepage nun unter/Mobile anstelle von/Mobile/Home befindet, da "Home" nun der implizit standardmäßige Controller Name für Mobile Seiten ist.

Beachten Sie, dass Sie durch Hinzufügen eines zweiten HomeController zu Ihrer Anwendung (d. h. dem mobilen Gerät, zusätzlich zum vorhandenen Desktop eines) Ihre reguläre Desktop-Startseite beschädigt haben. Es tritt ein Fehler mit dem Fehler "es*wurden mehrere Typen gefunden, die dem Controller mit dem Namen ' Home ' entsprechen*" ab. Um dieses Problem zu beheben, aktualisieren Sie Ihre Routing Konfiguration auf oberster Ebene (in Global.asax.cs), um anzugeben, dass Ihr Desktop-HomeController bei Mehrdeutigkeit Priorität haben sollte:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample9.cs)]

Der Fehler wird angezeigt, und die URL http:\/\/*yoursite*/erreicht die Desktop Homepage, und http:\/\/*yoursite*/Mobile/erreicht die mobile Homepage.

### <a name="redirecting-mobile-visitors-to-your-mobile-area"></a>Umleiten mobiler Besucher an Ihren mobilen Bereich

In ASP.NET MVC gibt es viele verschiedene Erweiterbarkeits Punkte. es gibt also viele Möglichkeiten, Umleitungs Logik einzufügen. Eine saubere Option ist das Erstellen eines Filter Attributs [redirectmobiledevicestomobilearea], das eine Umleitung ausführt, wenn die folgenden Bedingungen erfüllt sind:

1. Dies ist die erste Anforderung in der Sitzung des Benutzers (d. h. Session. IsNewSession ist true).
2. Die Anforderung stammt von einem mobilen Browser (d.h. Request. Browser. IsMobileDevice ist true).
3. Der Benutzer fordert im mobilen Bereich nicht bereits eine Ressource an (d. h., der *Pfad* Teil der URL beginnt nicht mit/Mobile).

Das in diesem Whitepaper enthaltene herunterladbare Beispiel enthält eine Implementierung dieser Logik. Sie wird als Autorisierungs Filter implementiert, der von "Autorität Attribute" abgeleitet ist. Dies bedeutet, dass Sie ordnungsgemäß funktionieren kann, auch wenn Sie die Ausgabe Zwischenspeicherung verwenden (andernfalls, wenn ein Desktop Besucher zum ersten Mal auf eine bestimmte URL zugreift, kann die Desktop Ausgabe zwischengespeichert und dann für nachfolgende Mobile Besucher).

Da es sich um einen Filter handelt, können Sie auswählen, ob Sie ihn auf bestimmte Controller und Aktionen anwenden möchten, z. b.

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample10.cs)]

… oder Sie können Sie auf alle Controller und Aktionen als globalen MVC 3- *Filter* in ihrer Global.asax.cs-Datei anwenden:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample11.cs)]

Das herunterladbare Beispiel veranschaulicht auch, wie Sie Unterklassen dieses Attributs erstellen können, die an bestimmte Speicherorte innerhalb Ihres mobilen Bereichs umgeleitet werden. Dies bedeutet beispielsweise Folgendes:

- Registrieren Sie einen globalen Filter, wie oben gezeigt, der standardmäßig Mobile Besucher an die mobile Homepage sendet.
- Wenden Sie außerdem einen speziellen [redirectmobiledevicestomobileproductpage]-Filter auf die Aktion "Produktanzeigen" an, die Mobile Besucher zur mobilen Version der von Ihnen angeforderten Produktseite führt.
- Wenden Sie auch andere spezielle Unterklassen des Filters auf bestimmte Aktionen an, indem Sie Mobile Besucher an die entsprechende Mobile Seite umleiten.

### <a name="configuring-forms-authentication-to-respect-your-mobile-pages"></a>Konfigurieren der Formular Authentifizierung zur Berücksichtigung ihrer mobilen Seiten

Wenn Sie die Formular Authentifizierung verwenden, sollten Sie beachten, dass ein Benutzer, der sich anmelden muss, den Benutzer automatisch an eine einzelne spezifische URL für das Anmelden (Log on) weiterleitet, die standardmäßig **/Account/Logon**lautet. Dies bedeutet, dass Mobile Benutzer möglicherweise zu Ihrer Desktop-Aktion "Anmelden" umgeleitet werden.

Um dieses Problem zu vermeiden, fügen Sie der Desktop Aktion "Anmelden" eine Logik hinzu, damit Mobile Benutzer erneut zu einer mobilen "Anmelden"-Aktion umgeleitet werden. Wenn Sie die standardmäßige ASP.NET-MVC-Anwendungs Vorlage verwenden, aktualisieren Sie die Anmelde Aktion von AccountController wie folgt:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample12.cs)]

… und implementieren Sie dann eine geeignete Mobile-spezifische Aktion zum Anmelden auf einem Controller namens AccountController in Ihrem mobilen Bereich.

### <a name="working-with-output-caching"></a>Arbeiten mit Ausgabe Caching

Wenn Sie den [OutputCache]-Filter verwenden, müssen Sie erzwingen, dass der Cache Eintrag je nach Gerätetyp variieren kann. Schreiben Sie z. b. Folgendes:

[!code-javascript[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample13.js)]

Fügen Sie dann die folgende Methode zu ihrer Global.asax.cs-Datei hinzu:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample14.cs)]

Dadurch wird sichergestellt, dass Mobile Besucher der Seite keine Ausgabe erhalten, die zuvor von einem Desktop Besucher in den Cache eingefügt wurde.

### <a name="a-working-example"></a>Ein funktionierendes Beispiel

Um diese Techniken in Aktion zu sehen, laden Sie [die Codebeispiele dieses Whitepaper](https://docs.microsoft.com/aspnet/mobile/overview)herunter. Das Beispiel enthält eine ASP.NET MVC 3 (Release Candidate)-Anwendung, die zur Unterstützung mobiler Geräte mithilfe der oben beschriebenen Methoden erweitert wurde.

## <a name="further-guidance-and-suggestions"></a>Weitere Anleitungen und Vorschläge

Die folgende Erläuterung gilt sowohl für Web Forms-als auch für MVC-Entwickler, die die in diesem Dokument behandelten Verfahren verwenden.

### <a name="enhancing-your-redirection-logic-using-51degreesmobi-foundation"></a>Verbessern der Umleitungs Logik mithilfe von 51Degrees.mobi Foundation

Die in diesem Dokument gezeigte Umleitungs Logik ist möglicherweise für Ihre Anwendung ausreichend, aber Sie funktioniert nicht, wenn Sie Sitzungen deaktivieren müssen, oder durch Mobile Browser, die Cookies ablehnen (diese können keine Sitzungen aufweisen), da Sie nicht wissen, ob eine bestimmte Anforderung der erste von diesem Besucher.

Sie haben bereits gelernt, wie das Open Source 51Degrees.mobi Foundation die Genauigkeit von ASP verbessern kann. Netzwerk-Browser Erkennung. Außerdem verfügt sie über eine integrierte Möglichkeit, Mobile Besucher an bestimmte Orte umzuleiten, die in der Datei "Web. config" konfiguriert sind. Sie kann ohne Abhängigkeit von ASP.NET-Sitzungen (und somit Cookies) funktionieren, indem ein temporäres Protokoll mit Hashes von den HTTP-Headern und IP-Adressen der Besucher gespeichert wird, sodass es weiß, ob jede Anforderung das erste Element eines bestimmten Vistors ist.

Das folgende Element, das dem Abschnitt "meftyone" der Datei "Web. config" hinzugefügt wird, leitet die erste Anforderung von einem erkannten mobilen Gerät an die Seite um ~/Mobile/default.aspx. Alle Anforderungen an Seiten im mobilen Ordner werden unabhängig vom Gerätetyp *nicht* umgeleitet. Wenn das Mobile Gerät für einen Zeitraum von 20 Minuten inaktiv ist, wird das Gerät vergessen, und nachfolgende Anforderungen werden als neue für die Umleitung behandelt.

[!code-xml[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample15.xml)]

Weitere Informationen finden Sie in der [51degrees.mobi Foundation-Dokumentation](https://github.com/51Degrees/dotNET-Device-Detection).

> [!NOTE]
> Sie *können* das 51Degrees.mobi Foundation-Umleitungs Feature für ASP.NET MVC-Anwendungen verwenden, aber Sie müssen ihre Umleitungs Konfiguration in Bezug auf einfache URLs definieren, nicht in Bezug auf Routing Parameter oder durch das Einfügen von MVC-Filtern auf Aktionen. Dies liegt daran, dass (zum Zeitpunkt des Schreibens) 51Degrees.mobi Foundation keine Filter oder Routing erkennt.

### <a name="disabling-transcoders-and-proxy-servers"></a>Deaktivieren von transcoders und Proxy Servern

Mobile Network-Operatoren haben zwei allgemeine Ziele bei der Herangehensweise an das Mobile Internet:

1. So viel relevante Inhalte wie möglich bereitstellen
2. Maximieren Sie die Anzahl der Kunden, die die eingeschränkte Funk Netzwerkbandbreite gemeinsam nutzen können.

Da die meisten Webseiten für große Bildschirme auf Desktop Größe und schnelle Breitbandverbindungen mit fester Linie entworfen wurden, verwenden viele Operatoren *Transcoder* oder *Proxy Server* , die Webinhalte dynamisch ändern. Sie können Ihr HTML-Markup oder-CSS in kleinere Bildschirme ändern (insbesondere für "featuretelefone", die nicht die Verarbeitungsleistung zum Verarbeiten komplexer Layouts haben), und Sie können Ihre Images neu komprimieren (wodurch die Qualität erheblich reduziert wird), um die Seiten Zustellungs Geschwindigkeit zu verbessern.

Wenn Sie jedoch eine Mobile optimierte Version Ihrer Website erstellt haben, möchten Sie wahrscheinlich nicht, dass der Netzwerk Bediener weitere Auswirkungen darauf hat. Sie können der Seite\_Lade Ereignis in einem beliebigen ASP.net Web Form die folgende Zeile hinzufügen:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample16.cs)]

Oder bei einem ASP.NET-MVC-Controller könnten Sie die folgende Methoden Überschreibung hinzufügen, sodass Sie für alle Aktionen auf diesem Controller gilt:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample17.cs)]

Die resultierende HTTP-Nachricht informiert W3C-kompatible Transcoder und Proxys daran, Inhalte nicht zu ändern. Natürlich gibt es keine Garantie dafür, dass die Operatoren mobiler Netzwerke diese Nachricht beachten werden.

### <a name="styling-mobile-pages-for-mobile-browsers"></a>Formatieren von mobilen Seiten für Mobile Browser

Es geht über den Rahmen dieses Dokuments hinaus, um ausführlich zu beschreiben, welche Arten von HTML-Markup ordnungsgemäß funktionieren oder welche webentwurfs Techniken die Nutzbarkeit auf bestimmten Geräten maximieren. Es liegt an Ihnen, ein ausreichend einfaches Layout zu finden, das für einen Bildschirm mit mobilen Größen optimiert ist, ohne unzuverlässige HTML-oder CSS-Positionierungs Tricks zu verwenden. Ein wichtiges Verfahren, das erwähnenswert ist, ist jedoch das *viewportmeta-Tag*.

Bestimmte moderne Mobile Browser zeigen Webseiten, die für Desktop Monitore vorgesehen sind, auf einem virtuellen Zeichenbereich an, der auch als "Viewport" bezeichnet wird (z. b. ist der virtuelle Viewport 980 Pixel breit auf iPhone und 850 Pixel breit in Opera Mobile). Skalieren Sie das Ergebnis nach unten, um es an die physischen Pixel des Bildschirms anzupassen. Der Benutzer kann dann diesen Viewport vergrößern und schwenken. Dies hat den Vorteil, dass der Browser die Seite im vorgesehenen Layout anzeigen kann. er hat jedoch auch den Nachteil, dass er das Zoomen und Schwenken erzwingt, was für den Benutzer ungeeignet ist. Wenn Sie für Mobilgeräte entwickeln, ist es besser, einen schmalen Bildschirm zu entwerfen, damit keine Zoom-oder horizontale Bildläufe erforderlich sind.

Eine Möglichkeit, dem mobilen Browser mitzuteilen, wie breit der Viewport sein sollte, ist über das nicht dem Standard entsprechende *Viewport* -Meta-Tag. Wenn Sie z. b. Folgendes zum Head-Abschnitt der Seite hinzufügen,

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample18.html)]

… durch die Unterstützung von Smartphone-Browsern wird die Seite auf einem 480-Pixel-weiten virtuellen Zeichenbereich angezeigt. Dies bedeutet, dass die Prozentsätze in Bezug auf diese 480-Pixel-Breite, nicht die Standardbreite des Viewports interpretiert werden, wenn Ihre HTML-Elemente ihre breiten in Prozent definieren. Daher ist es weniger wahrscheinlich, dass der Benutzer horizontal Zoomen und Schwenken muss – das mobile Browsen erheblich verbessert.

Wenn Sie möchten, dass die Breite des Viewports mit den physischen Pixeln des Geräts identisch ist, können Sie Folgendes angeben:

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample19.html)]

Damit dies ordnungsgemäß funktioniert, müssen Elemente nicht explizit gezwungen werden, diese Breite zu überschreiten (z. b. mithilfe eines *Width* -Attributs oder einer CSS-Eigenschaft), da der Browser andernfalls gezwungen wird, einen größeren Viewport zu verwenden. Siehe auch: [Weitere Informationen zum nicht dem Standard folgenden viewporttag](https://developer.apple.com/library/safari/#documentation/AppleApplications/Reference/SafariHTMLRef/Articles/MetaTags.html).

Die meisten modernen Smartphones unterstützen die *duale Ausrichtung*: Sie können im hoch-oder Querformat gehalten werden. Daher ist es wichtig, keine Annahmen über die Bildschirmbreite in Pixel zu treffen. Nehmen Sie nicht einmal an, dass die Bildschirmbreite fest ist, da der Benutzer sein Gerät erneut ausrichten kann, während Sie sich auf der Seite befinden.

Ältere Windows Mobile-und BlackBerry-Geräte akzeptieren möglicherweise auch die folgenden Meta-Tags im Seitenkopf, um Sie darüber zu informieren, dass der Inhalt für mobile Geräte optimiert wurde und daher nicht transformiert werden sollte.

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample20.html)]

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Eine Liste der Emulatoren und Simulatoren für mobile Geräte, die Sie zum Testen Ihrer Mobile ASP.NET-Webanwendung verwenden können, finden Sie auf der Seite [simulieren beliebter mobiler Geräte zu Test](../mobile/device-simulators.md)Zwecken.

## <a name="credits"></a>Guthaben

- Primärer Autor: Steven Sanderson
- Reviewer/zusätzliche inhaltswriter: James Rosewell, Mikael Söderström, Scott Hanselman, Scott Hunter
