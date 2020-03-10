---
uid: whitepapers/aspnet4/overview
title: Übersicht über ASP.NET 4 und Visual Studio 2010 Webentwicklung | Microsoft-Dokumentation
author: rick-anderson
description: Dieses Dokument bietet eine Übersicht über viele der neuen Features für ASP.net, die in the.NET Framework 4 und in Visual Studio 2010 enthalten sind.
ms.author: riande
ms.date: 02/10/2010
ms.assetid: d7729af4-1eda-4ff2-8b61-dbbe4fc11d10
msc.legacyurl: /whitepapers/aspnet4
msc.type: content
ms.openlocfilehash: ecde48f6bd88ee5f569bfeb8b70c26a50bc869c2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78511431"
---
# <a name="aspnet-4-and-visual-studio-2010-web-development-overview"></a>Webentwicklung mit ASP.NET 4 und Visual Studio 2010 – Übersicht

> Dieses Dokument bietet eine Übersicht über viele der neuen Features für ASP.net, die in the.NET Framework 4 und in Visual Studio 2010 enthalten sind.
> 
> [Dieses Whitepaper herunterladen](https://download.microsoft.com/download/7/1/A/71A105A9-89D6-4201-9CC5-AD6A3B7E2F22/ASP_NET_4_and_Visual_Studio_2010_Web_Development_Overview.pdf)

**Contents**

**[Kerndienste](#0.2__Toc253429238 "_Toc253429238")**  
[Refactoring der Datei "Web. config"](#0.2__Toc253429239 "_Toc253429239")  
[Erweiterbares Ausgabe Caching](#0.2__Toc253429240 "_Toc253429240")  
[Webanwendungen automatisch starten](#0.2__Toc253429241 "_Toc253429241")  
[Dauerhaftes Umleiten einer Seite](#0.2__Toc253429242 "_Toc253429242")  
[Verkleinern des Sitzungs Zustands](#0.2__Toc253429243 "_Toc253429243")  
[Erweitern des Bereichs zulässiger URLs](#0.2__Toc253429244 "_Toc253429244")  
[Erweiterbare Anforderungs Validierung](#0.2__Toc253429245 "_Toc253429245")  
[Objekt Zwischenspeicherung und Erweiterbarkeit von Objekt Zwischenspeichern](#0.2__Toc253429246 "_Toc253429246")  
[Erweiterbare HTML-, URL-und HTTP-Header Codierung](#0.2__Toc253429247 "_Toc253429247")  
[Leistungsüberwachung für einzelne Anwendungen in einem einzelnen Arbeitsprozess](#0.2__Toc253429248 "_Toc253429248")  
[Zielplattform](#0.2__Toc253429249 "_Toc253429249")

**[Sucher](#0.2__Toc253429250 "_Toc253429250")**  
[in Web Forms und MVC enthaltene jQuery](#0.2__Toc253429251 "_Toc253429251")  
[Content Delivery Network Unterstützung](#0.2__Toc253429252 "_Toc253429252")  
[Explizite ScriptManager-Skripts](#0.2__Toc253429253 "_Toc253429253")

**[Web Forms](#0.2__Toc253429256 "_Toc253429256")**  
[Festlegen von Meta-Tags mit den Eigenschaften Page. MetaKeywords und Page. MetaDescription](#0.2__Toc253429257 "_Toc253429257")  
[Aktivieren des Ansichts Zustands für einzelne Steuerelemente](#0.2__Toc253429258 "_Toc253429258")  
[Änderungen an Browser Funktionen](#0.2__Toc253429259 "_Toc253429259")  
[Routing in ASP.NET 4](#0.2__Toc253429260 "_Toc253429260")  
[Festlegen von Client-IDs](#0.2__Toc253429261 "_Toc253429261")  
[Persistente Zeilenauswahl in Daten Steuerelementen](#0.2__Toc253429262 "_Toc253429262")  
[ASP.net-Diagramm Steuerelement](#0.2__Toc253429263 "_Toc253429263")  
[Filtern von Daten mit dem QueryExtender-Steuerelement](#0.2__Toc253429264 "_Toc253429264")  
[HTML-codierte Code Ausdrücke](#0.2__Toc253429265 "_Toc253429265")  
[Änderungen an der Projektvorlage](#0.2__Toc253429266 "_Toc253429266")  
[CSS-Verbesserungen](#0.2__Toc253429267 "_Toc253429267")  
[Ausblenden von div-Elementen um verborgene Felder](#0.2__Toc253429268 "_Toc253429268")  
[Rendern einer äußeren Tabelle für Steuerelemente mit Vorlagen](#0.2__Toc253429269 "_Toc253429269")  
[Erweiterungen des ListView-Steuer Elements](#0.2__Toc253429270 "_Toc253429270")  
[Verbesserungen der Check boxlist-und RadioButton List-Steuerelemente](#0.2__Toc253429271 "_Toc253429271")  
[Verbesserungen im Menü](#0.2__Toc253429272 "_Toc253429272")  
[Assistent und der Assistent für das auflisteuserwizard 56](#0.2__Toc253429273 "_Toc253429273")

**[ASP.NET MVC](#0.2__Toc253429274 "_Toc253429274")**  
[Unterstützung von Bereichen](#0.2__Toc253429275 "_Toc253429275")  
[Unterstützung der Daten Annotation-Attribut Validierung](#0.2__Toc253429276 "_Toc253429276")  
[Auf Vorlagen basierende Hilfsprogramme](#0.2__Toc253429277 "_Toc253429277")

**[dynamische Daten](#0.2__Toc253429278 "_Toc253429278")**  
[Aktivieren von dynamische Daten für vorhandene Projekte](#0.2__Toc253429279 "_Toc253429279")  
[Deklarative DynamicDataManager-Steuerelement Syntax](#0.2__Toc253429280 "_Toc253429280")  
[Entitäts Vorlagen](#0.2__Toc253429281 "_Toc253429281")  
[Neue Feld Vorlagen für URLs und e-Mail-Adressen](#0.2__Toc253429282 "_Toc253429282")  
[Erstellen von Verknüpfungen mit dem DynamicHyperLink-Steuerelement](#0.2__Toc253429283 "_Toc253429283")  
[Unterstützung für Vererbung im Datenmodell](#0.2__Toc253429284 "_Toc253429284")  
[Unterstützung für m:n-Beziehungen (nur Entity Framework)](#0.2__Toc253429285 "_Toc253429285")  
[Neue Attribute zum Steuern der Anzeige und Unterstützung von Enumerationen](#0.2__Toc253429286 "_Toc253429286")  
[Erweiterte Unterstützung für Filter](#0.2__Toc253429287 "_Toc253429287")

**[Verbesserungen der Webentwicklung in Visual Studio 2010](#0.2__Toc253429288 "_Toc253429288")**  
[Verbesserte CSS-Kompatibilität](#0.2__Toc253429289 "_Toc253429289")  
[HTML-und JavaScript-Code Ausschnitte](#0.2__Toc253429290 "_Toc253429290")  
[JavaScript-IntelliSense-Erweiterungen](#0.2__Toc253429291 "_Toc253429291")

**[Webanwendungs Bereitstellung mit Visual Studio 2010](#0.2__Toc253429292 "_Toc253429292")**  
[Webverpackung](#0.2__Toc253429293 "_Toc253429293")  
[Web. config-Transformation](#0.2__Toc253429294 "_Toc253429294")  
[Daten Bank Bereitstellung](#0.2__Toc253429295 "_Toc253429295")  
[One-Click-Veröffentlichung für Webanwendungen](#0.2__Toc253429296 "_Toc253429296")  
[Ressourcen](#0.2__Toc253429297 "_Toc253429297")

**[ERS](#0.2__Toc253429298 "_Toc253429298")**

<a id="0.2__Toc224729018"></a><a id="0.2__Toc253429238"></a><a id="0.2__Toc243304612"></a>

## <a name="core-services"></a>Kerndienste

ASP.NET 4 führt eine Reihe von Features ein, die Kern ASP.NET Dienste wie Ausgabe Caching und Sitzungs Zustands Speicher verbessern.

<a id="0.2__Toc243304613"></a><a id="0.2__Toc253429239"></a><a id="0.2__Toc224729019"></a>

### <a name="webconfig-file-refactoring"></a>Refactoring der Datei "Web. config"

Die `Web.config`-Datei, die die Konfiguration für eine Webanwendung enthält, wurde in den letzten Versionen der .NET Framework erheblich vergrößert, da neue Features hinzugefügt wurden, wie z. b. AJAX, Routing und Integration in IIS 7. Dies hat das Konfigurieren oder Starten neuer Webanwendungen ohne ein Tool wie Visual Studio erschwert. In. NET Framework 4 wurden die Haupt Konfigurationselemente in die `machine.config`-Datei verschoben, und Anwendungen erben diese Einstellungen jetzt. Dadurch kann die `Web.config` Datei in ASP.NET 4-Anwendungen entweder leer sein oder nur die folgenden Zeilen enthalten, die für Visual Studio angeben, auf welche Version des Frameworks die Anwendung ausgerichtet ist:

[!code-xml[Main](overview/samples/sample1.xml)]

<a id="0.2__Toc253429240"></a><a id="0.2__Toc243304614"></a>

### <a name="extensible-output-caching"></a>Erweiterbares Ausgabe Caching

Seit der Veröffentlichung von ASP.NET 1,0 hat der Ausgabe Cache Entwicklern ermöglicht, die generierte Ausgabe von Seiten, Steuerelementen und HTTP-Antworten im Arbeitsspeicher zu speichern. Bei nachfolgenden Webanforderungen kann ASP.net Inhalt schneller verarbeiten, indem die generierte Ausgabe aus dem Arbeitsspeicher abgerufen wird, anstatt die Ausgabe von Grund auf neu zu generieren. Allerdings hat dieser Ansatz eine Einschränkung – der generierte Inhalt muss immer im Arbeitsspeicher gespeichert werden. auf Servern mit hohem Datenverkehr kann der von der Ausgabe Zwischenspeicherung genutzte Arbeitsspeicher mit Speicheranforderungen anderer Teile einer Webanwendung konkurrieren.

ASP.NET 4 fügt der Ausgabe Zwischenspeicherung einen Erweiterbarkeits Punkt hinzu, der es Ihnen ermöglicht, einen oder mehrere benutzerdefinierte Ausgabe Cache Anbieter zu konfigurieren. Ausgabe Cache Anbieter können einen beliebigen Speichermechanismus verwenden, um HTML-Inhalte beizubehalten. Dies ermöglicht das Erstellen benutzerdefinierter Ausgabe Cache Anbieter für verschiedene Persistenzmechanismen, die lokale oder Remote Datenträger, cloudspeicher und verteilte Cache Module umfassen können.

Sie erstellen einen benutzerdefinierten Ausgabe Cache Anbieter als eine Klasse, die vom neuen *System. Web. Caching. OutputCacheProvider* -Typ abgeleitet wird. Anschließend können Sie den Anbieter in der `Web.config`-Datei konfigurieren, indem Sie den neuen *Anbieter* unter Abschnitt des *OutputCache* -Elements verwenden, wie im folgenden Beispiel gezeigt:

[!code-xml[Main](overview/samples/sample2.xml)]

Standardmäßig verwenden alle HTTP-Antworten, gerenderten Seiten und Steuerelemente in ASP.NET 4 den in-Memory-Ausgabe Cache, wie im vorherigen Beispiel gezeigt, wobei das *defaultProvider* -Attribut auf AspNetInternalProvider festgelegt ist. Sie können den Standard-Ausgabe Cache Anbieter ändern, der für eine Webanwendung verwendet wird, indem Sie einen anderen Anbieter Namen für *defaultProvider*angeben.

Außerdem können Sie verschiedene Ausgabe Cache Anbieter pro Kontrolle und pro Anforderung auswählen. Die einfachste Möglichkeit, einen anderen Ausgabe Cache Anbieter für verschiedene Webbenutzer Steuerelemente auszuwählen, besteht darin, diese deklarativ zu verwenden, indem Sie das neue *providerName* -Attribut in einer Control-Direktive verwenden, wie im folgenden Beispiel gezeigt:

[!code-aspx[Main](overview/samples/sample3.aspx)]

Die Angabe eines anderen Ausgabe Cache Anbieters für eine HTTP-Anforderung erfordert etwas mehr Arbeit. Anstatt den Anbieter deklarativ anzugeben, überschreiben Sie die neue *getouputcacheprovidername* -Methode in der `Global.asax`-Datei, um Programm gesteuert anzugeben, welcher Anbieter für eine bestimmte Anforderung verwendet werden soll. Das folgende Beispiel zeigt die dazu erforderliche Vorgehensweise.

[!code-csharp[Main](overview/samples/sample4.cs)]

Durch das Hinzufügen der Erweiterbarkeit von Ausgabe Cache Anbietern zu ASP.NET 4 können Sie jetzt aggressivere und intelligentere Strategien für die Ausgabe Zwischenspeicherung für Ihre Websites verfolgen. Beispielsweise ist es jetzt möglich, die "Top 10"-Seiten einer Website im Speicher zwischenzuspeichern, während Seiten zwischengespeichert werden, die geringeren Datenverkehr auf dem Datenträger erhalten. Alternativ können Sie jede beliebige Kombination für eine gerenderte Seite zwischenspeichern, aber einen verteilten Cache verwenden, damit die Arbeitsspeicher Nutzung von Front-End-Webservern ausgelagert wird.

<a id="0.2__Toc224729020"></a><a id="0.2__Toc253429241"></a><a id="0.2__Toc243304615"></a>

### <a name="auto-start-web-applications"></a>Webanwendungen automatisch starten

Einige Webanwendungen müssen große Datenmengen laden oder eine aufwändige Initialisierungs Verarbeitung durchführen, bevor die erste Anforderung bedient wird. In früheren Versionen von ASP.net mussten Sie in diesen Situationen benutzerdefinierte Ansätze entwickeln, um eine ASP.NET-Anwendung zu "reaktivieren" und dann während der *Anwendung\_Load* -Methode in der `Global.asax` Datei Initialisierungs Code auszuführen.

Ein neues *Skalierbarkeits Feature namens Autostart* , das dieses Szenario direkt adressiert, ist verfügbar, wenn ASP.NET 4 unter IIS 7,5 unter Windows Server 2008 R2 ausgeführt wird. Die Funktion für automatisches Starten bietet einen kontrollierten Ansatz zum Starten eines Anwendungs Pools, zum Initialisieren einer ASP.NET-Anwendung und zum anschließenden akzeptieren von HTTP-Anforderungen.

> [!NOTE] 
> 
> IIS-anwendungsaufwärm Modul für IIS 7,5
> 
> Das IIS-Team hat die erste Beta-Testversion des anwendungsaufwärm Moduls für IIS 7,5 veröffentlicht. Dadurch wird das Aufwärmen Ihrer Anwendungen noch einfacher als zuvor beschrieben. Anstatt benutzerdefinierten Code zu schreiben, geben Sie die URLs der Ressourcen an, die ausgeführt werden sollen, bevor die Webanwendung Anforderungen aus dem Netzwerk akzeptiert. Diese Aufwärmphase tritt während des Starts des IIS-Diensts auf (wenn Sie den IIS-Anwendungs Pool als *alwaysrunning*konfiguriert haben) und wenn ein IIS-Arbeitsprozess wieder verwendet wird. Während der Wiederverwendung führt der alte IIS-Arbeitsprozess die Ausführung von Anforderungen fort, bis der neu erzeugte Arbeitsprozess vollständig bereinigt ist, sodass bei Anwendungen keine Unterbrechungen oder andere Probleme aufgrund von nicht Primed-Caches auftreten. Beachten Sie, dass dieses Modul mit einer beliebigen Version von ASP.NET funktioniert, beginnend mit Version 2,0.
> 
> Weitere Informationen finden Sie auf der IIS.NET-Website unter [anwendungsaufwärm](https://www.iis.net/extensions/applicationwarmup%20on%20the%20IIS.net) Phase. Eine exemplarische Vorgehensweise, in der die Verwendung der Aufwärm Funktion veranschaulicht wird, finden Sie unter [Getting Started with the IIS 7,5 Application warm-up Module](https://www.iis.net/learn/manage) auf der IIS.NET-Website.

Um die Funktion zum automatischen Starten zu verwenden, legt ein IIS-Administrator einen Anwendungs Pool in IIS 7,5 so fest, dass er automatisch gestartet wird, indem die folgende Konfiguration in der `applicationHost.config`-Datei verwendet wird:

[!code-xml[Main](overview/samples/sample5.xml)]

Da ein einzelner Anwendungs Pool mehrere Anwendungen enthalten kann, geben Sie einzelne Anwendungen an, die automatisch gestartet werden sollen, indem Sie die folgende Konfiguration in der `applicationHost.config`-Datei verwenden:

[!code-xml[Main](overview/samples/sample6.xml)]

Wenn ein IIS 7,5-Server mit einem Kaltstart oder einem einzelnen Anwendungs Pool wieder verwendet wird, verwendet IIS 7,5 die Informationen in der `applicationHost.config`-Datei, um zu bestimmen, welche Webanwendungen automatisch gestartet werden müssen. Für jede Anwendung, die für den automatischen Start markiert ist, sendet IIS 7.5 eine Anforderung an ASP.NET 4, um die Anwendung in einem Zustand zu starten, in dem die Anwendung vorübergehend keine HTTP-Anforderungen akzeptiert. Wenn er sich in diesem Zustand befindet, instanziiert ASP.NET den Typ, der vom *serviceautostartprovider* -Attribut definiert wird (wie im vorherigen Beispiel gezeigt), und ruft seinen öffentlichen Einstiegspunkt auf.

Sie erstellen einen verwalteten Autostarttyp mit dem erforderlichen Einstiegspunkt, indem Sie die *iprocesshostpreloadclient* -Schnittstelle implementieren, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](overview/samples/sample7.cs)]

Nachdem der Initialisierungs Code in der *Preload* -Methode ausgeführt und die-Methode zurückgegeben wurde, ist die ASP.NET-Anwendung bereit für die Verarbeitung von Anforderungen.

Mit dem automatischen Start von IIS 0,5 und ASP.NET 4 verfügen Sie nun über einen klar definierten Ansatz, um eine teure Anwendungs Initialisierung durchzuführen, bevor die erste HTTP-Anforderung verarbeitet wird. Beispielsweise können Sie die neue Funktion zum automatischen Starten verwenden, um eine Anwendung zu initialisieren und dann einem Lastenausgleich zu signalisieren, dass die Anwendung initialisiert wurde und bereit ist, den HTTP-Datenverkehr zu akzeptieren.

<a id="0.2__Toc224729021"></a><a id="0.2__Toc253429242"></a><a id="0.2__Toc243304616"></a>

### <a name="permanently-redirecting-a-page"></a>Dauerhaftes Umleiten einer Seite

Es ist üblich, dass Webanwendungen Seiten und andere Inhalte im Laufe der Zeit verschieben, was zu einer Ansammlung veralteter Verknüpfungen in Suchmaschinen führen kann. In ASP.net haben Entwickler in der Regel Anforderungen an alte URLs verarbeitet, indem Sie mit der *Response. Redirect* -Methode eine Anforderung an die neue URL weiterleiten. Allerdings gibt die *Umleitungs* Methode eine HTTP 302-Antwort (temporäre Umleitung) aus, die zu einem zusätzlichen http-Roundtrip führt, wenn Benutzer versuchen, auf die alten URLs zuzugreifen.

ASP.NET 4 Fügt eine neue *redirectpermanente* -Hilfsmethode hinzu, mit der Sie problemlos HTTP 301-verschoderte Antworten ausgeben können, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](overview/samples/sample8.cs)]

Suchmaschinen und andere Benutzer-Agents, die permanente Umleitungen erkennen, speichern die neue URL, die dem Inhalt zugeordnet ist. Dadurch entfällt der unnötige Roundtrip, den der Browser für temporäre Umleitungen vornimmt.

<a id="0.2__Toc224729022"></a><a id="0.2__Toc253429243"></a><a id="0.2__Toc243304617"></a>

### <a name="shrinking-session-state"></a>Verkleinern des Sitzungs Zustands

ASP.net bietet zwei Standardoptionen zum Speichern des Sitzungs Zustands in einer Webfarm: einen Sitzungs Zustands Anbieter, der einen prozessübergreifenden Sitzungs Zustands Server aufruft, und einen Sitzungs Zustands Anbieter, der Daten in einer Microsoft SQL Server Datenbank speichert. Da beide Optionen das Speichern von Zustandsinformationen außerhalb des Arbeitsprozesses einer Webanwendung einschließen, muss der Sitzungszustand serialisiert werden, bevor er an den Remote Speicher gesendet wird. Je nachdem, wie viele Informationen ein Entwickler im Sitzungszustand speichert, kann die Größe der serialisierten Daten sehr groß werden.

ASP.NET 4 führt eine neue Komprimierungs Option für beide Arten von Out-of-Process-Sitzungs Zustands Anbietern ein. Wenn die im folgenden Beispiel gezeigte Konfigurationsoption *compressionaktivierte* auf *true*festgelegt ist, wird der serialisierte Sitzungszustand von ASP.NET mithilfe der .NET Framework *System. IO. Compression. GZipStream* -Klasse komprimiert (und dekomprimiert).

[!code-xml[Main](overview/samples/sample9.xml)]

Durch das einfache Hinzufügen des neuen Attributs zur `Web.config` Datei können Anwendungen mit freien CPU-Zyklen auf Webservern eine erhebliche Reduzierung der Größe der serialisierten Sitzungszustandsdaten erzielen.

<a id="0.2__Toc253429244"></a><a id="0.2__Toc243304618"></a>

### <a name="expanding-the-range-of-allowable-urls"></a>Erweitern des Bereichs zulässiger URLs

ASP.NET 4 führt neue Optionen zum Erweitern der Größe von Anwendungs-URLs ein. Frühere Versionen von ASP.net beschränkte URL-Pfadlängen auf 260 Zeichen, basierend auf dem NTFS-Dateipfad Limit. In ASP.NET 4 haben Sie die Möglichkeit, diese Beschränkung entsprechend Ihren Anwendungen zu erhöhen (oder zu verringern), indem Sie zwei neue *HttpRuntime* -Konfigurations Attribute verwenden. Das folgende Beispiel zeigt diese neuen Attribute.

[!code-xml[Main](overview/samples/sample10.xml)]

Um längere oder kürzere Pfade zuzulassen (der Teil der URL, der nicht das Protokoll, den Servernamen und die Abfrage Zeichenfolge enthält), ändern Sie das Attribut " *[maxurllength](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.maxurllength.aspx)* ". Um längere oder kürzere Abfrage Zeichenfolgen zuzulassen, ändern Sie den Wert des *[maxquerystringlength](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.maxquerystringlength.aspx)* -Attributs.

ASP.NET 4 ermöglicht außerdem das Konfigurieren der Zeichen, die von der URL-Zeichen Überprüfung verwendet werden. Wenn ASP.net ein ungültiges Zeichen im Pfadteil einer URL findet, wird die Anforderung zurückgewiesen, und es wird ein HTTP 400-Fehler ausgegeben. In früheren Versionen von ASP.net waren die URL-Zeichen Überprüfungen auf einen festgelegten Satz von Zeichen beschränkt. In ASP.NET 4 können Sie den Satz gültiger Zeichen mithilfe des neuen *requestpathinvalidcharacter* -Attributs des *HttpRuntime* -Konfigurations Elements anpassen, wie im folgenden Beispiel gezeigt:

[!code-xml[Main](overview/samples/sample11.xml)]

Standardmäßig definiert das *requestpathinvalidcharacter* -Attribut acht Zeichen als ungültig. (In der Zeichenfolge, die *RequestPathInvalidCharacters* zugewiesen wird, werden die Zeichen "kleiner als (&lt;)", "größer als" (&gt;) und "kaufmännisches Zeichen" (&amp;) codiert, da es sich bei der `Web.config` Datei um eine XML-Datei handelt.) Sie können die Anzahl der ungültigen Zeichen nach Bedarf anpassen.

> [!NOTE]
> Hinweis ASP.NET 4 weist immer URL-Pfade zurück, die Zeichen im ASCII-Bereich von 0x00 bis 0x1F enthalten, da es sich um ungültige URL-Zeichen handelt, die in RFC 2396 des IETF ([http://www.ietf.org/rfc/rfc2396.txt](http://www.ietf.org/rfc/rfc2396.txt)) definiert sind. Bei Windows Server-Versionen, die IIS 6 oder höher ausführen, lehnt der http. sys-Protokoll Gerätetreiber URLs mit diesen Zeichen automatisch ab.

<a id="0.2__Toc253429245"></a><a id="0.2__Toc243304619"></a>

### <a name="extensible-request-validation"></a>Erweiterbare Anforderungs Validierung

ASP.net Request Validation durchsucht eingehende HTTP-Anforderungs Daten nach Zeichen folgen, die häufig in Cross-Site Scripting (XSS)-Angriffen verwendet werden. Wenn potenzielle XSS-Zeichen folgen gefunden werden, wird die Fehler verdächtige Zeichenfolge von der Anforderungs Validierung gekennzeichnet und ein Fehler zurückgegeben. Die integrierte Anforderungs Validierung gibt nur dann einen Fehler zurück, wenn die am häufigsten verwendeten Zeichen folgen gefunden werden, die in XSS-Angriffen verwendet werden. Vorherige Versuche, die XSS-Validierung aggressiver zu machen, ergaben zu viele falsch positive Ergebnisse. Kunden möchten jedoch möglicherweise eine Überprüfung anfordern, die aggressiver ist, oder Sie möchten möglicherweise die XSS-Prüfungen für bestimmte Seiten oder für bestimmte Arten von Anforderungen absichtlich lockern.

In ASP.NET 4 wurde das Anforderungs Validierungs Feature erweiterbar gemacht, sodass Sie benutzerdefinierte Logik für die Anforderungs Validierung verwenden können. Um die Anforderungs Validierung zu erweitern, erstellen Sie eine Klasse, die vom neuen *System. Web. util. RequestValidator* -Typ abgeleitet wird, und konfigurieren Sie die Anwendung (im *HttpRuntime* -Abschnitt der `Web.config` Datei), um den benutzerdefinierten Typ zu verwenden. Das folgende Beispiel zeigt, wie Sie eine benutzerdefinierte Anforderungs Validierungs Klasse konfigurieren:

[!code-xml[Main](overview/samples/sample12.xml)]

Das neue *requestvalidationtype* -Attribut erfordert eine standardmäßige .NET Framework typbezeichnerzeichenfolge, die die Klasse angibt, die benutzerdefinierte Anforderungs Validierung bereitstellt. Für jede Anforderung ruft ASP.NET den benutzerdefinierten Typ auf, um die einzelnen eingehenden HTTP-Anforderungs Daten zu verarbeiten. Die eingehende URL, alle HTTP-Header (sowohl Cookies als auch benutzerdefinierte Header) und der Entitäts Text sind für die Überprüfung durch eine benutzerdefinierte Anforderungs Validierungs Klasse verfügbar, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](overview/samples/sample13.cs)]

In Fällen, in denen Sie keine eingehenden HTTP-Daten überprüfen möchten, kann die Anforderungs Validierungs Klasse einen Fallback durchführen, um die ASP.net-standardmäßige Anforderungs Validierung durch einfaches Aufrufen von *Base auszuführen. IsValidRequestString.*

<a id="0.2__Toc253429246"></a><a id="0.2__Toc243304620"></a>

### <a name="object-caching-and-object-caching-extensibility"></a>Objekt Zwischenspeicherung und Erweiterbarkeit von Objekt Zwischenspeichern

Seit der ersten Version enthält ASP.net einen leistungsfähigen Speicher internen Objekt Cache (*System. Web. Caching. Cache*). Die Cache Implementierung ist so beliebt, dass Sie in nicht-Webanwendungen verwendet wurde. Allerdings ist es für eine Windows Forms-oder WPF-Anwendung nicht schwierig, einen Verweis auf `System.Web.dll` einzuschließen, um nur den ASP.NET-Objekt Cache verwenden zu können.

Um die Zwischenspeicherung für alle Anwendungen verfügbar zu machen, wird in .NET Framework 4 eine neue Assembly, ein neuer Namespace, einige Basis Typen und eine konkrete Cache Implementierung eingeführt. Die neue `System.Runtime.Caching.dll`-Assembly enthält eine neue Caching-API im *System. Runtime. Caching* -Namespace. Der-Namespace enthält zwei Kernsätze von Klassen:

- Abstrakte Typen, die die Grundlage zum Erstellen beliebiger Art von benutzerdefinierter Cache Implementierung darstellen.
- Eine konkrete Implementierung des in-Memory-Objekt Caches (die *System. Runtime. Caching. MemoryCache* -Klasse).

Die neue *MemoryCache* -Klasse wird im ASP.NET-Cache eng modelliert und nutzt einen Großteil der internen Cache-Engine-Logik mit ASP.net. Obwohl die öffentlichen Caching-APIs in *System. Runtime. Caching* aktualisiert wurden, um die Entwicklung von benutzerdefinierten Caches zu unterstützen, finden Sie, wenn Sie das ASP.net- *Cache* Objekt verwendet haben, vertraute Konzepte in den neuen APIs.

Eine ausführliche Erörterung der neuen *MemoryCache* -Klasse und der unterstützenden Basis-APIs erfordern ein gesamtes Dokument. Im folgenden Beispiel erhalten Sie jedoch eine Vorstellung davon, wie die neue Cache-API funktioniert. Das Beispiel wurde für eine Windows Forms Anwendung geschrieben, ohne dass eine Abhängigkeit von `System.Web.dll`besteht.

[!code-csharp[Main](overview/samples/sample14.cs)]

<a id="0.2__Toc253429247"></a><a id="0.2__Toc243304621"></a>

### <a name="extensible-html-url-and-http-header-encoding"></a>Erweiterbare HTML-, URL-und HTTP-Header Codierung

In ASP.NET 4 können Sie benutzerdefinierte Codierungs Routinen für die folgenden allgemeinen Text Codierungs Aufgaben erstellen:

- HTML-Codierung.
- URL-Codierung.
- HTML-Attribut Codierung.
- Codieren von ausgehenden HTTP-Headern.

Sie können einen benutzerdefinierten Encoder erstellen, indem Sie vom neuen *System. Web. util. httpcoder* -Typ ableiten und dann ASP.net so konfigurieren, dass der benutzerdefinierte Typ im *HttpRuntime* -Abschnitt der `Web.config` Datei verwendet wird, wie im folgenden Beispiel gezeigt:

[!code-xml[Main](overview/samples/sample15.xml)]

Nachdem ein benutzerdefinierter Encoder konfiguriert wurde, ruft ASP.NET automatisch die benutzerdefinierte Codierungs Implementierung auf, wenn öffentliche Codierungs Methoden der Klassen *System. Web. HttpUtility* oder *System. Web. HttpServerUtility* aufgerufen werden. Dies ermöglicht es einem Teil eines Webentwicklungs Teams, einen benutzerdefinierten Encoder zu erstellen, der eine aggressive Zeichencodierung implementiert, während der Rest des Webentwicklungs Teams weiterhin die öffentlichen ASP.net-Codierungs-APIs verwendet. Wenn Sie einen benutzerdefinierten Encoder im *HttpRuntime* -Element zentral konfigurieren, wird sichergestellt, dass alle Text Codierungs Aufrufe aus den öffentlichen ASP.net Encoding-APIs über den benutzerdefinierten Encoder weitergeleitet werden.

<a id="0.2__Toc253429248"></a><a id="0.2__Toc243304622"></a>

### <a name="performance-monitoring-for-individual-applications-in-a-single-worker-process"></a>Leistungsüberwachung für einzelne Anwendungen in einem einzelnen Arbeitsprozess

Um die Anzahl der Websites zu erhöhen, die auf einem einzelnen Server gehostet werden können, führen viele Hoster mehrere ASP.NET-Anwendungen in einem einzelnen Arbeitsprozess aus. Wenn jedoch mehrere Anwendungen einen einzelnen freigegebenen Arbeitsprozess verwenden, ist es für Server Administratoren schwierig, eine einzelne Anwendung zu identifizieren, bei der Probleme auftreten.

ASP.NET 4 nutzt neue Ressourcen Überwachungsfunktionen, die von der CLR eingeführt wurden. Um diese Funktionalität zu aktivieren, können Sie den folgenden XML-Konfigurations Ausschnitt der `aspnet.config` Konfigurationsdatei hinzufügen.

[!code-xml[Main](overview/samples/sample16.xml)]

> [!NOTE]
> Beachten Sie, dass sich die `aspnet.config` Datei in dem Verzeichnis befindet, in dem die .NET Framework installiert ist. Dabei handelt es sich nicht um die `Web.config` Datei.

Wenn das *appdomainresourcemonitoring* -Feature aktiviert wurde, sind in der Leistungs Kategorie "ASP.NET Applications" zwei neue Leistungsindikatoren verfügbar: *% Managed Processor Time* und *verwalteter verwendeter Arbeitsspeicher*. Beide Leistungsindikatoren verwenden das neue Feature "CLR-Anwendungs Domänen Ressourcenverwaltung", um die geschätzte CPU-Zeit und die Auslastung des verwalteten Arbeitsspeichers einzelner ASP.NET-Anwendungen zu verfolgen. Daher haben Administratoren mit ASP.NET 4 jetzt eine präzisetere Ansicht des Ressourcenverbrauchs einzelner Anwendungen, die in einem einzelnen Arbeitsprozess ausgeführt werden.

<a id="0.2__Toc253429249"></a><a id="0.2__Toc243304623"></a>

### <a name="multi-targeting"></a>Festlegung von Zielversionen

Sie können eine Anwendung erstellen, die eine bestimmte Version des .NET Framework als Ziel hat. In ASP.NET 4 können Sie mithilfe eines neuen Attributs im *Kompilierungs* Element der `Web.config` Datei auf die .NET Framework 4 und höher abzielen. Wenn Sie die .NET Framework 4 explizit als Ziel angeben und optionale Elemente in die `Web.config` Datei einschließen, z. b. die Einträge für *System. CodeDom*, müssen diese Elemente für die .NET Framework 4 korrekt sein. (Wenn Sie den .NET Framework 4 nicht explizit als Ziel verwenden, wird das Ziel Framework aus dem fehlenden Eintrag in der `Web.config` Datei abgeleitet.)

Im folgenden Beispiel wird gezeigt, wie das *TargetFramework* -Attribut im *Kompilierungs* Element der `Web.config` Datei verwendet wird.

[!code-xml[Main](overview/samples/sample17.xml)]

Beachten Sie Folgendes, wenn Sie auf eine bestimmte Version des .NET Framework abzielen:

- In einem .NET Framework 4-Anwendungs Pool nimmt das ASP.NET-Buildsystem die .NET Framework 4 als Ziel an, wenn die `Web.config` Datei das *TargetFramework* -Attribut nicht enthält oder wenn die `Web.config` Datei fehlt. (Möglicherweise müssen Sie Codierungs Änderungen an Ihrer Anwendung vornehmen, damit Sie unter der .NET Framework 4 ausgeführt wird.)
- Wenn Sie das *TargetFramework* -Attribut einschließen und das *System. CodeDom* -Element in der `Web.config`-Datei definiert ist, muss diese Datei die richtigen Einträge für die .NET Framework 4 enthalten.
- Wenn Sie den *ASPNET\_-Compilerbefehl* verwenden, um die Anwendung Vorkompilieren (z. b. in einer Buildumgebung), müssen Sie die korrekte Version des *ASPNET\_-compilerbefehls* für das Ziel Framework verwenden. Verwenden Sie den Compiler, der mit dem .NET Framework 2,0 (%windir%\Microsoft.NET\Framework\v2.0.50727) geliefert wurde, um für die .NET Framework 3,5 und frühere Versionen zu kompilieren. Verwenden Sie den Compiler, der im Lieferumfang von .NET Framework 4 enthalten ist, um Anwendungen zu kompilieren, die mithilfe dieses Frameworks erstellt wurden, oder
- Zur Laufzeit verwendet der Compiler die neuesten Frameworkassemblys, die auf dem Computer installiert sind (und somit im GAC). Wenn eine spätere Aktualisierung des Frameworks erfolgt (z. b. Wenn eine hypothetische Version 4,1 installiert ist), können Sie Funktionen in der neueren Version des Frameworks verwenden, obwohl das *TargetFramework* -Attribut eine niedrigere Version (z. b. 4,0) als Ziel hat. (Zur Entwurfszeit in Visual Studio 2010 oder bei Verwendung des *ASPNET\_-compilerbefehls* verursachen die Verwendung neuer Funktionen des Frameworks jedoch Compilerfehler.)

<a id="0.2__Toc224729023"></a><a id="0.2__Toc253429250"></a><a id="0.2__Toc243304624"></a>

## <a name="ajax"></a>Ajax

<a id="0.2__Toc253429251"></a><a id="0.2__Toc243304625"></a>

### <a name="jquery-included-with-web-forms-and-mvc"></a>in Web Forms und MVC enthaltene jQuery

Die Visual Studio-Vorlagen für Web Forms und MVC enthalten die Open Source-Bibliothek jQuery. Wenn Sie eine neue Website oder ein neues Projekt erstellen, wird ein Skript Ordner erstellt, der die folgenden drei Dateien enthält:

- jQuery-1.4.1. js – die lesbare, nicht minierte Version der jQuery-Bibliothek.
- jQuery-14.1. min. js – die minierte Version der jQuery-Bibliothek.
- jQuery-1.4.1-vsdoc. js – die IntelliSense-Dokumentations Datei für die jQuery-Bibliothek.

Schließen Sie die nicht minierte Version von jQuery ein, während Sie eine Anwendung entwickeln. Schließen Sie die minierte Version von jQuery für Produktionsanwendungen ein.

Beispielsweise wird auf der folgenden Web Forms Seite veranschaulicht, wie Sie mithilfe von jQuery die Hintergrundfarbe der ASP.net-Textfeld-Steuerelemente auf gelb ändern können, wenn Sie den Fokus haben.

[!code-aspx[Main](overview/samples/sample18.aspx)]

<a id="0.2__Toc253429252"></a><a id="0.2__Toc243304626"></a>

### <a name="content-delivery-network-support"></a>Content Delivery Network Unterstützung

Mit dem Microsoft AJAX Content Delivery Network (CDN) können Sie Ihren Webanwendungen problemlos ASP.NET AJAX-und jQuery-Skripts hinzufügen. Beispielsweise können Sie mit der Verwendung der jQuery-Bibliothek beginnen, indem Sie Ihrer Seite ein `<script>`-Tag hinzufügen, das wie folgt auf AJAX.Microsoft.com zeigt:

[!code-html[Main](overview/samples/sample19.html)]

Wenn Sie das Microsoft AJAX CDN nutzen, können Sie die Leistung Ihrer AJAX-Anwendungen erheblich verbessern. Der Inhalt des Microsoft AJAX CDN wird auf Servern auf der ganzen Welt zwischengespeichert. Außerdem ermöglicht das Microsoft AJAX CDN Browser die Wiederverwendung von zwischengespeicherten JavaScript-Dateien für Websites, die sich in verschiedenen Domänen befinden.

Der Microsoft AJAX-Content Delivery Network unterstützt SSL (HTTPS) für den Fall, dass Sie eine Webseite mithilfe der Secure Sockets Layer bedienen müssen.

Implementieren Sie einen Fall Back, wenn das CDN nicht verfügbar ist. Testen Sie den Fallback.

Weitere Informationen zum Microsoft AJAX CDN finden Sie auf der folgenden Website:

[https://www.asp.net/ajaxlibrary/CDN.ashx](../../ajax/cdn/overview.md)

Der ASP.net ScriptManager unterstützt das Microsoft AJAX CDN. Durch einfaches Festlegen einer Eigenschaft, der enablecdn-Eigenschaft, können Sie alle ASP.NET Framework-JavaScript-Dateien aus dem CDN abrufen:

[!code-aspx[Main](overview/samples/sample20.aspx)]

Nachdem Sie die enablecdn-Eigenschaft auf den Wert true festgelegt haben, ruft das ASP.NET Framework alle ASP.NET Framework-JavaScript-Dateien aus dem CDN ab, einschließlich aller JavaScript-Dateien, die für die Validierung und Update Panel verwendet werden. Das Festlegen dieser Eigenschaft kann sich negativ auf die Leistung Ihrer Webanwendung auswirken.

Sie können den CDN-Pfad für Ihre eigenen JavaScript-Dateien mithilfe des WebResource-Attributs festlegen. Die neue cdnpath-Eigenschaft gibt den Pfad zum CDN an, der verwendet wird, wenn Sie die enablecdn-Eigenschaft auf den Wert "true" festlegen:

[!code-csharp[Main](overview/samples/sample21.cs)]

<a id="0.2__Toc253429253"></a><a id="0.2__Toc243304627"></a>

### <a name="scriptmanager-explicit-scripts"></a>Explizite ScriptManager-Skripts

Wenn Sie in der Vergangenheit den ASP.net ScriptManager verwendet haben, mussten Sie die gesamte monolithische ASP.NET AJAX-Bibliothek laden. Wenn Sie die neue ScriptManager. ajaxframeworkmode-Eigenschaft nutzen, können Sie genau steuern, welche Komponenten der ASP.NET AJAX-Bibliothek geladen werden, und nur die Komponenten der ASP.NET AJAX-Bibliothek laden, die Sie benötigen.

Die ScriptManager. ajaxframeworkmode-Eigenschaft kann auf die folgenden Werte festgelegt werden:

- Aktiviert: gibt an, dass das ScriptManager-Steuerelement automatisch die Skriptdatei "mikrosoftajax. js" enthält, bei der es sich um eine kombinierte Skriptdatei jedes kernframeworkskripts handelt (Legacy Verhalten).
- Deaktiviert: gibt an, dass alle Microsoft AJAX-Skriptfunktionen deaktiviert sind und dass das ScriptManager-Steuerelement nicht automatisch auf Skripts verweist.
- Explizit: gibt an, dass Sie explizit Skript Verweise auf einzelne frameworkkernskriptdateien einschließen, die für die Seite erforderlich sind, und dass Verweise auf die Abhängigkeiten eingeschlossen werden, die für die einzelnen Skriptdateien erforderlich sind.

Wenn Sie z. b. die ajaxframeworkmode-Eigenschaft auf den Wert "explizit" festlegen, können Sie die jeweiligen ASP.NET AJAX-Komponenten Skripts angeben, die Sie benötigen:

[!code-aspx[Main](overview/samples/sample22.aspx)]

<a id="0.2__The_DataView_Control"></a><a id="0.2__The_DataContext_and"></a><a id="0.2__Refactoring_the_Microsoft"></a><a id="0.2__Toc224729032"></a><a id="0.2__Toc253429256"></a><a id="0.2__Toc243304630"></a>

## <a name="web-forms"></a>Web Forms

Web Forms ist seit der Veröffentlichung von ASP.NET 1,0 ein Kern Feature in ASP.net. In diesem Bereich sind viele Verbesserungen für ASP.NET 4 enthalten, einschließlich der folgenden:

- Die Möglichkeit, *Meta* -Tags festzulegen.
- Mehr Kontrolle über den Ansichts Zustand.
- Einfachere Möglichkeiten, mit Browserfunktionen zu arbeiten.
- Unterstützung für die Verwendung von ASP.NET-Routing mit Web Forms.
- Mehr Kontrolle über generierte IDs.
- Die Möglichkeit, die ausgewählten Zeilen in Daten Steuerelementen beizubehalten.
- Mehr Kontrolle über gerendertes HTML in den Steuerelementen *FormView* und *ListView* .
- Filtern der Unterstützung für Datenquellen-Steuerelemente.

<a id="0.2__Toc224729033"></a><a id="0.2__Toc253429257"></a><a id="0.2__Toc243304631"></a>

### <a name="setting-meta-tags-with-the-pagemetakeywords-and-pagemetadescription-properties"></a>Festlegen von Meta-Tags mit den Eigenschaften Page. MetaKeywords und Page. MetaDescription

ASP.NET 4 fügt der *Page* -Klasse, den *metaschlüsselwörtern* und der *MetaDescription*, zwei Eigenschaften hinzu. Diese beiden Eigenschaften stellen entsprechende *Meta* -Tags auf der Seite dar, wie im folgenden Beispiel gezeigt:

[!code-aspx[Main](overview/samples/sample23.aspx)]

Diese beiden Eigenschaften funktionieren auf die gleiche Weise wie die *Title* -Eigenschaft der Seite. Die folgenden Regeln gelten:

1. Wenn im *Head* -Element keine *Meta* -Tags vorhanden sind, die den Eigenschaftsnamen entsprechen (d. h. Name = "Keywords" für *Page. MetaKeywords* und Name = "Description" für *Page. MetaDescription*, d. h., dass diese Eigenschaften nicht festgelegt wurden), werden die *Meta* -Tags der Seite hinzugefügt, wenn Sie gerendert wird.
2. Wenn bereits *Meta* -Tags mit diesen Namen vorhanden sind, fungieren diese Eigenschaften als Get-und Set-Methoden für den Inhalt der vorhandenen Tags.

Sie können diese Eigenschaften zur Laufzeit festlegen, sodass Sie den Inhalt aus einer Datenbank oder einer anderen Quelle erhalten können, und mit dem Sie die Tags dynamisch festlegen können, um zu beschreiben, wofür eine bestimmte Seite vorgesehen ist.

Sie können auch die *Schlüsselwörter* und *Beschreibungs* Eigenschaften in der *@ Page* -Direktive am Anfang des Web Forms Seiten Markups festlegen, wie im folgenden Beispiel gezeigt:

[!code-aspx[Main](overview/samples/sample24.aspx)]

Dadurch wird der Inhalt des *Meta* -Tags (sofern vorhanden) überschrieben, der bereits auf der Seite deklariert wurde.

Der Inhalt des Beschreibungs- *Meta* -Tags wird zum Verbessern der Suchlisten Vorschau in Google verwendet. (Ausführliche Informationen finden Sie unter [verbessern von Code Ausschnitten mit einer Beschreibung der Metabeschreibung](https://googlewebmastercentral.blogspot.com/2007/09/improve-snippets-with-meta-description.html) im Blog von Google Webmaster Central.) Google und Windows Live Search verwenden nicht die Inhalte der Schlüsselwörter, aber auch andere Suchmaschinen. Weitere Informationen finden Sie unter [Meta Keywords-Ratschläge](http://www.searchengineguide.com/richard-ball/meta-keywords-a.php) auf der Such-Engine-Handbuch-Website.

Diese neuen Eigenschaften sind eine einfache Funktion, Sie ersparen sich jedoch, diese manuell hinzuzufügen oder eigenen Code zu schreiben, um die *Metatags* zu erstellen.

<a id="0.2__Toc224729034"></a><a id="0.2__Toc253429258"></a><a id="0.2__Toc243304632"></a>

### <a name="enabling-view-state-for-individual-controls"></a>Aktivieren des Ansichts Zustands für einzelne Steuerelemente

Standardmäßig ist der Ansichts Zustand für die Seite aktiviert. das Ergebnis ist, dass jedes Steuerelement auf der Seite möglicherweise den Ansichts Zustand speichert, auch wenn es für die Anwendung nicht erforderlich ist. Ansichts Zustandsdaten sind im Markup enthalten, das eine Seite generiert, und erhöhen die Zeitspanne, die benötigt wird, um eine Seite an den Client zu senden und zurückzustellen. Das Speichern eines größeren Ansichts Zustands als notwendig kann zu erheblichen Leistungseinbußen führen. In früheren Versionen von ASP.net konnten Entwickler den Ansichts Zustand für einzelne Steuerelemente deaktivieren, um die Seitengröße zu verringern, mussten dies jedoch explizit für einzelne Steuerelemente durchführen. In ASP.NET 4 enthalten Webserver Steuerelemente eine *ViewStateMode* -Eigenschaft, mit der Sie den Ansichts Zustand standardmäßig deaktivieren und dann nur für die Steuerelemente aktivieren können, die auf der Seite erforderlich sind.

Die *viewstatuemode* -Eigenschaft nimmt eine Enumeration mit drei Werten an: *aktiviert*, *deaktiviert*und *erben*. *Aktiviert aktiviert* den Ansichts Zustand für dieses Steuerelement und für alle untergeordneten Steuerelemente, die als *erben* festgelegt oder für die nichts festgelegt ist. *Deaktiviert* deaktiviert den Ansichts Zustand, und *erben* gibt an, dass das Steuerelement die *ViewStateMode* -Einstellung aus dem übergeordneten Steuerelement verwendet.

Im folgenden Beispiel wird gezeigt, wie die *viewstatuemode* -Eigenschaft funktioniert. Das Markup und der Code für die Steuerelemente auf der folgenden Seite enthalten Werte für die *viewstatuemode* -Eigenschaft:

[!code-aspx[Main](overview/samples/sample25.aspx)]

Wie Sie sehen können, deaktiviert der Code den Ansichts Zustand für das Placeholder1-Steuerelement. Das untergeordnete Label1-Steuerelement*erbt* diesen Eigenschafts Wert (erben ist der Standardwert für *ViewStateMode* für Steuerelemente) und speichert daher keinen Ansichts Zustand. Im PlaceHolder2-Steuerelement ist *ViewStateMode* auf " *aktiviert*" festgelegt. Daher erbt Label2 diese Eigenschaft und speichert den Ansichts Zustand. Wenn die Seite zum ersten Mal geladen wird, wird die *Text* -Eigenschaft der beiden *Label* -Steuerelemente auf die Zeichenfolge "[dynamicvalue]" festgelegt.

Die Auswirkung dieser Einstellungen besteht darin, dass die folgende Ausgabe im Browser angezeigt wird, wenn die Seite das erste Mal geladen wird:

Deaktiviert `: [DynamicValue]`

Aktiviert:`[DynamicValue]`

Nach einem Postback wird jedoch die folgende Ausgabe angezeigt:

Deaktiviert `: [DeclaredValue]`

Aktiviert:`[DynamicValue]`

Das Label1-Steuerelement (dessen *viewstatuemode* -Wert auf " *deaktiviert*" festgelegt ist) hat den Wert, auf den er im Code festgelegt wurde, nicht beibehalten. Allerdings hat das Label2-Steuerelement (dessen *ViewStateMode* -Wert auf " *aktiviert*" festgelegt ist) seinen Zustand beibehalten.

Sie können auch *VIEWSTATUS EMODE* in der *@ Page* -Direktive festlegen, wie im folgenden Beispiel gezeigt:

[!code-aspx[Main](overview/samples/sample26.aspx)]

Die *Seiten* Klasse ist nur ein weiteres Steuerelement. Er fungiert als übergeordnetes Steuerelement für alle anderen Steuerelemente auf der Seite. Der Standardwert von *VIEWSTATUS EMODE* ist für Instanzen von *Page* *aktiviert* . Da Steuerelemente standardmäßig *erben*, erben die Steuerelemente den *aktivierten* Eigenschafts Wert, es sei denn, Sie legen *VIEWSTATUS EMODE* auf Seiten-oder Steuerelement Ebene

Der Wert der *ViewStateMode* -Eigenschaft bestimmt, ob der Ansichts Zustand nur beibehalten wird, wenn die *EnableViewState* -Eigenschaft auf *true*festgelegt ist. Wenn die *EnableViewState* -Eigenschaft auf *false*festgelegt ist, wird der Ansichts Zustand nicht beibehalten, auch wenn *ViewStateMode* auf " *aktiviert*" festgelegt ist.

Eine gute Verwendung für diese Funktion ist die Verwendung von *contentplachalter* -Steuerelementen in Masterseiten, bei denen *ViewStateMode* für die Master Seite auf *deaktiviert* festgelegt und dann einzeln für *contentplachalter* -Steuerelemente aktiviert werden kann, die wiederum Steuerelemente enthalten, die den Ansichts Zustand erfordern.

<a id="0.2__Toc224729035"></a><a id="0.2__Toc253429259"></a><a id="0.2__Toc243304633"></a>

### <a name="changes-to-browser-capabilities"></a>Änderungen an Browser Funktionen

ASP.NET bestimmt die Funktionen des Browsers, den ein Benutzer zum Durchsuchen der Website mithilfe eines Features namens *Browserfunktionen*verwendet. Browser Funktionen werden durch das *httpbrowserfunktionalitäten* -Objekt dargestellt, das von der *Request. Browser* -Eigenschaft verfügbar gemacht wird. Beispielsweise können Sie das *httpbrowsersettings* -Objekt verwenden, um zu bestimmen, ob der Typ und die Version des aktuellen Browsers eine bestimmte Version von JavaScript unterstützen. Oder Sie können das *httpbrowserfunktionalitäten* -Objekt verwenden, um zu bestimmen, ob die Anforderung von einem mobilen Gerät stammt.

Das *httpbrowserfunktionalitäten* -Objekt wird durch einen Satz von Browser Definitions Dateien gesteuert. Diese Dateien enthalten Informationen zu den Funktionen bestimmter Browser. In ASP.NET 4 wurden diese Browser Definitions Dateien so aktualisiert, dass Sie Informationen zu kürzlich eingeführten Browsern und Geräten wie Google Chrome, Research in Motion BlackBerry Smartphones und Apple iPhone enthalten.

In der folgenden Liste werden neue Browser Definitions Dateien angezeigt:

- *BlackBerry. Browser*
- *Chrome. Browser*
- *Default. Browser*
- *Firefox. Browser*
- *Gateway. Browser*
- *generischer. Browser*
- *IE. Browser*
- *Iemobile. Browser*
- *iPhone. Browser*
- *Opera. Browser*
- *Safari. Browser*

#### <a name="using-browser-capabilities-providers"></a>Verwenden von Browser Funktions Anbietern

In ASP.NET Version 3,5 Service Pack 1 können Sie die Funktionen eines Browsers wie folgt definieren:

- Auf Computer Ebene erstellen oder aktualisieren Sie eine `.browser` XML-Datei im folgenden Ordner:

- [!code-console[Main](overview/samples/sample27.cmd)]

- Nachdem Sie die Browserfunktion definiert haben, führen Sie den folgenden Befehl über die Visual Studio-Eingabeaufforderung aus, um die Assembly mit den Browserfunktionen neu zu erstellen und dem GAC hinzuzufügen:

- [!code-console[Main](overview/samples/sample28.cmd)]

- Für eine einzelne Anwendung erstellen Sie eine `.browser`-Datei im `App_Browsers` Ordner der Anwendung.

Diese Ansätze erfordern, dass Sie XML-Dateien ändern. bei Änderungen auf Computer Ebene müssen Sie die Anwendung neu starten, nachdem Sie den Prozess ASPNET\_regbrowsers. exe ausgeführt haben.

ASP.NET 4 enthält ein Feature, das als *Anbieter von Browserfunktionen*bezeichnet wird. Wie der Name bereits vermuten lässt, können Sie einen Anbieter erstellen, mit dem Sie wiederum ihren eigenen Code zum Bestimmen von Browserfunktionen verwenden können.

In der Praxis definieren Entwickler häufig keine benutzerdefinierten Browserfunktionen. Browser Dateien sind schwer zu aktualisieren, der Aktualisierungsprozess ist recht kompliziert, und die XML-Syntax für `.browser` Dateien kann komplex sein und definieren. Dieser Prozess ist viel einfacher, wenn eine gängige Browser Definitions Syntax oder eine Datenbank mit aktuellen Browser Definitionen oder sogar ein Webdienst für eine solche Datenbank vorhanden wäre. Die neue Funktion "Browser Funktions Anbieter" macht diese Szenarios für Entwickler von Drittanbietern möglich und praktikabel.

Es gibt zwei Hauptansätze für die Verwendung der neuen ASP.NET 4-Browserfunktionen-Anbieter Funktion: Erweiterung der Funktionalität der ASP.net-Browserfunktionen oder vollständig Ersetzung. In den folgenden Abschnitten wird zuerst beschrieben, wie Sie die Funktionalität ersetzen und dann erweitern.

#### <a name="replacing-the-aspnet-browser-capabilities-functionality"></a>Ersetzen der Funktionen der ASP.net-Browser Funktionen

Gehen Sie folgendermaßen vor, um die Definition der ASP.net-Browserfunktionen vollständig zu ersetzen:

1. Erstellen Sie eine Anbieter Klasse, die von *HttpCapabilitiesProvider* abgeleitet wird und die *getbrowserfunktionalitäten* -Methode überschreibt, wie im folgenden Beispiel gezeigt: 

    [!code-csharp[Main](overview/samples/sample29.cs)]

    Der Code in diesem Beispiel erstellt ein neues *httpbrowsercapability* -Objekt, das nur die Funktion mit dem Namen Browser angibt und diese Funktion auf mycustombrowser festlegt.
2. Registrieren Sie den Anbieter bei der Anwendung. 

    Um einen Anbieter mit einer Anwendung verwenden zu können, müssen Sie das *Provider* -Attribut dem *browserCaps* -Abschnitt in den `Web.config`-oder `Machine.config`-Dateien hinzufügen. (Sie können auch die Anbieter Attribute in einem *Location* -Element für bestimmte Verzeichnisse in der Anwendung definieren, z. b. in einem Ordner für ein bestimmtes mobiles Gerät.) Im folgenden Beispiel wird gezeigt, wie das *Provider* -Attribut in einer Konfigurationsdatei festgelegt wird:

    [!code-xml[Main](overview/samples/sample30.xml)]

    Eine weitere Möglichkeit zum Registrieren der neuen Browser Funktionsdefinition ist die Verwendung von Code, wie im folgenden Beispiel gezeigt:

    [!code-csharp[Main](overview/samples/sample31.cs)]

    Dieser Code muss im *Anwendungs\_Start* Ereignis der `Global.asax` Datei ausgeführt werden. Jede Änderung an der *browsercapabilitiesprovider* -Klasse muss erfolgen, bevor Code in der Anwendung ausgeführt wird, um sicherzustellen, dass der Cache in einem gültigen Zustand für das aufgelöste *HttpCapabilitiesBase* -Objekt verbleibt.

#### <a name="caching-the-httpbrowsercapabilities-object"></a>Zwischenspeichern des httpbrowserfunktionalitäten-Objekts

Das vorangehende Beispiel weist ein Problem auf, d. h., der Code wird jedes Mal ausgeführt, wenn der benutzerdefinierte Anbieter aufgerufen wird, um das *httpbrowserfunktionalitäten* -Objekt zu erhalten. Dies kann bei jeder Anforderung mehrmals vorkommen. Im Beispiel wird der Code für den Anbieter nicht viel unterstützt. Wenn der Code in Ihrem benutzerdefinierten Anbieter jedoch bedeutende Aufgaben ausführt, um das *httpbrowserfunktionalitäten* -Objekt zu erhalten, kann dies die Leistung beeinträchtigen. Um dies zu verhindern, können Sie das *httpbrowserfunktionalitäten* -Objekt Zwischenspeichern. Folgen Sie diesen Schritten:

1. Erstellen Sie eine Klasse, die von *HttpCapabilitiesProvider*abgeleitet wird, wie im folgenden Beispiel gezeigt: 

    [!code-csharp[Main](overview/samples/sample32.cs)]

    Im Beispiel generiert der Code einen Cache Schlüssel durch Aufrufen einer benutzerdefinierten buildcachekey-Methode und ruft die Zeitspanne ab, die zwischengespeichert werden soll, indem eine benutzerdefinierte getcachetime-Methode aufgerufen wird. Der Code fügt dann das aufgelöste *httpbrowserfunktionalitäten* -Objekt dem Cache hinzu. Das Objekt kann aus dem Cache abgerufen und für nachfolgende Anforderungen wieder verwendet werden, die den benutzerdefinierten Anbieter verwenden.
2. Registrieren Sie den Anbieter bei der Anwendung, wie im vorherigen Verfahren beschrieben.

#### <a name="extending-aspnet-browser-capabilities-functionality"></a>Erweitern der Funktionen der ASP.net-Browser Funktionen

Im vorherigen Abschnitt wurde das Erstellen eines neuen *httpbrowserfunktionalitäten* -Objekts in ASP.NET 4 beschrieben. Sie können auch die Funktionen der ASP.net-Browserfunktionen erweitern, indem Sie neuen Browser Funktionsdefinitionen hinzufügen, die bereits in ASP.net vorhanden sind. Dies ist ohne Verwendung der XML-Browser Definitionen möglich. Im folgenden Verfahren wird die Vorgehensweise veranschaulicht.

1. Erstellen Sie eine Klasse, die von *httpcapabilitiesevaluator* abgeleitet wird und die *getbrowserfunktionalitäten* -Methode überschreibt, wie im folgenden Beispiel gezeigt: 

    [!code-csharp[Main](overview/samples/sample33.cs)]

    Dieser Code verwendet zunächst die Funktionen der ASP.net-Browserfunktionen, um den Browser zu identifizieren. Wenn jedoch kein Browser auf der Grundlage der in der Anforderung definierten Informationen identifiziert wird (d. h., wenn die *Browser* -Eigenschaft des *httpbrowserfunktionalitäten* -Objekts die Zeichenfolge "unknown" ist), ruft der Code den benutzerdefinierten Anbieter (mybrowsercapabilitiesevaluator) auf, um den Browser zu identifizieren.
2. Registrieren Sie den Anbieter wie im vorherigen Beispiel beschrieben bei der Anwendung.

#### <a name="extending-browser-capabilities-functionality-by-adding-new-capabilities-to-existing-capabilities-definitions"></a>Erweitern der Funktionalität von Browser Funktionen durch Hinzufügen neuer Funktionen zu vorhandenen Funktionsdefinitionen

Zusätzlich zum Erstellen eines benutzerdefinierten Browser Definitions Anbieters und zum dynamischen Erstellen neuer Browser Definitionen können Sie vorhandene Browser Definitionen mit zusätzlichen Funktionen erweitern. Dies ermöglicht es Ihnen, eine Definition zu verwenden, die sich in der Nähe Ihrer gewünschten Elemente befindet, aber nur wenige Funktionen enthält. Gehen Sie dazu folgendermaßen vor.

1. Erstellen Sie eine Klasse, die von *httpcapabilitiesevaluator* abgeleitet wird und die *getbrowserfunktionalitäten* -Methode überschreibt, wie im folgenden Beispiel gezeigt: 

    [!code-csharp[Main](overview/samples/sample34.cs)]

    Der Beispielcode erweitert die vorhandene ASP.net *httpcapabilitiesevaluator* -Klasse und ruft das *httpbrowserabilisobjekt* ab, das mit der aktuellen Anforderungsdefinition übereinstimmt, indem der folgende Code verwendet wird:

    [!code-csharp[Main](overview/samples/sample35.cs)]

    Der Code kann dann eine Funktion für diesen Browser hinzufügen oder ändern. Es gibt zwei Möglichkeiten, eine neue Browserfunktion anzugeben:

    - Fügen Sie dem *IDictionary* -Objekt ein Schlüssel-Wert-Paar hinzu, das von der *Funktionen* -Eigenschaft des *HttpCapabilitiesBase* -Objekts verfügbar gemacht wird. Im vorherigen Beispiel fügt der Code eine Funktion mit dem Namen "multiberühren" mit dem Wert " *true*" hinzu.
    - Legen Sie vorhandene Eigenschaften des *HttpCapabilitiesBase* -Objekts fest. Im vorherigen Beispiel legt der Code die *Frames* -Eigenschaft auf *true*fest. Diese Eigenschaft ist einfach ein Accessor für das *IDictionary* -Objekt, das von der *Funktionen* -Eigenschaft verfügbar gemacht wird. 

        > [!NOTE]
        > Beachten Sie, dass dieses Modell für jede Eigenschaft von *httpbrowserfunktionen*gilt, einschließlich der Steuerelement Adapter.
2. Registrieren Sie den Anbieter bei der Anwendung, wie im vorherigen Verfahren beschrieben.

<a id="0.2__Toc224729036"></a><a id="0.2__Toc253429260"></a><a id="0.2__Toc243304634"></a>

### <a name="routing-in-aspnet-4"></a>Routing in ASP.NET 4

ASP.NET 4 bietet integrierte Unterstützung für die Verwendung von Routing mit Web Forms. Mithilfe von Routing können Sie eine Anwendung so konfigurieren, dass Sie Anforderungs-URLs akzeptiert, die nicht physischen Dateien zugeordnet sind. Stattdessen können Sie das Routing verwenden, um URLs zu definieren, die für Benutzer aussagekräftig sind und die bei der Suche-Engine-Optimierung (SEO) für Ihre Anwendung unterstützen können. Beispielsweise könnte die URL für eine Seite, auf der die Produktkategorien in einer vorhandenen Anwendung angezeigt werden, wie im folgenden Beispiel aussehen:

[!code-console[Main](overview/samples/sample36.cmd)]

Mithilfe des Routings können Sie die Anwendung so konfigurieren, dass Sie die folgende URL zum Rendering der gleichen Informationen akzeptiert:

[!code-console[Main](overview/samples/sample37.cmd)]

Das Routing ist ab ASP.NET 3,5 SP1 verfügbar. (Ein Beispiel für das Verwenden des Routings in ASP.NET 3,5 SP1 finden Sie im Artikel [Verwenden von Routing mit WebForms](http://haacked.com/archive/2008/03/11/using-routing-with-webforms.aspx "Titel dieses Eintrags.") im Blog von Phil Haack.) ASP.NET 4 enthält jedoch einige Features, die die Verwendung des Routings vereinfachen, einschließlich der folgenden:

- Die *PageRouteHandler* -Klasse, bei der es sich um einen einfachen HTTP-Handler handelt, den Sie beim Definieren von Routen verwenden. Die-Klasse übergibt Daten an die Seite, an die die Anforderung weitergeleitet wird.
- Die neuen Eigenschaften *HttpRequest. RequestContext* und *Page. RouteData* (Hierbei handelt es sich um einen Proxy für das *HttpRequest. RequestContext. RouteData* -Objekt). Diese Eigenschaften erleichtern den Zugriff auf Informationen, die von der Route weitergeleitet werden.
- Die folgenden neuen Ausdrucks-Generatoren werden in " *System. Web. Compilation. routeurlexpressionbuilder* " und " *System. Web. Compilation. routevalueexpressionbuilder*" definiert:
- *RouteUrl*bietet eine einfache Möglichkeit, eine URL zu erstellen, die einer Routen-URL in einem ASP.NET-Server Steuerelement entspricht.
- *Routevalue*, das eine einfache Möglichkeit zum Extrahieren von Informationen aus dem *routecontext* -Objekt bietet.
- Die *RouteParameter* -Klasse vereinfacht das Übergeben von Daten, die in einem *routecontext* -Objekt enthalten sind, an eine Abfrage für ein Datenquellen-Steuerelement (ähnlich wie bei [*FormParameter*](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formparameter.aspx)).

#### <a name="routing-for-web-forms-pages"></a>Routing für Web Forms Seiten

Im folgenden Beispiel wird gezeigt, wie eine Web Forms Route mithilfe der neuen *MapPageRoute* -Methode der *Route* -Klasse definiert wird:

[!code-csharp[Main](overview/samples/sample38.cs)]

ASP.NET 4 führt die *MapPageRoute* -Methode ein. Das folgende Beispiel entspricht der im vorherigen Beispiel gezeigten searchroute-Definition, verwendet jedoch die *PageRouteHandler* -Klasse.

[!code-csharp[Main](overview/samples/sample39.cs)]

Der Code im Beispiel ordnet die Route einer physischen Seite zu (in der ersten Route zu `~/search.aspx`). Die erste Routen Definition gibt auch an, dass der Parameter mit dem Namen Suchbegriff aus der URL extrahiert und an die Seite übergeben werden soll.

Die *MapPageRoute* -Methode unterstützt die folgenden Methoden Überladungen:

- *MapPageRoute (String routename, String RouteUrl, String PhysicalFile, bool checkphysicalurlaccess)*
- *MapPageRoute (String routename, String RouteUrl, String PhysicalFile, bool checkphysicalurlaccess, RouteValueDictionary defaults)*
- *MapPageRoute (String routename, String RouteUrl, String PhysicalFile, bool checkphysicalurlaccess, RouteValueDictionary defaults, RouteValueDictionary-Einschränkungen)*

Der *checkphysicalurlaccess* -Parameter gibt an, ob die Route die Sicherheits Berechtigungen für die physische Seite, an die weitergeleitet wird (in diesem Fall Search. aspx), und die Berechtigungen für die eingehende URL (in diesem Fall search/{SearchTerm}) überprüfen soll. Wenn der Wert von *checkphysicalurlaccess* auf *false*festgelegt ist, werden nur die Berechtigungen der eingehenden URL überprüft. Diese Berechtigungen werden in der `Web.config`-Datei mit folgenden Einstellungen definiert:

[!code-xml[Main](overview/samples/sample40.xml)]

In der Beispielkonfiguration wird der Zugriff auf die physische Seite `search.aspx` für alle Benutzer verweigert, mit Ausnahme derjenigen, die in der Administrator Rolle sind. Wenn der *checkphysicalurlaccess* -Parameter auf *true* festgelegt ist (Dies ist der Standardwert), dürfen nur Administrator Benutzer auf die URL/search/{SearchTerm} zugreifen, da die physische Seite Search. aspx auf die Benutzer in dieser Rolle beschränkt ist. Wenn *checkphysicalurlaccess* auf *false* festgelegt ist und die Website so konfiguriert ist, wie im vorherigen Beispiel gezeigt, können alle authentifizierten Benutzer auf die URL zugreifen/search/{SearchTerm}.

#### <a name="reading-routing-information-in-a-web-forms-page"></a>Lesen von Routing Informationen auf einer Web Forms Seite

Im Code der Web Forms physischen Seite können Sie auf die Informationen zugreifen, die das Routing aus der URL (oder anderen Informationen, die ein anderes Objekt zum *RouteData* -Objekt hinzugefügt hat) extrahiert hat, indem Sie zwei neue Eigenschaften verwenden: *HttpRequest. RequestContext* und *Page. RouteData*. (*Page. RouteData* umschließt *HttpRequest. RequestContext. RouteData*.) Im folgenden Beispiel wird gezeigt, wie *Page. RouteData*verwendet wird.

[!code-csharp[Main](overview/samples/sample41.cs)]

Der Code extrahiert den Wert, der für den Suchbegriff-Parameter übergeben wurde, wie in der vorherigen Beispiel Route definiert. Beachten Sie die folgende Anforderungs-URL:

[!code-console[Main](overview/samples/sample42.cmd)]

Wenn diese Anforderung erfolgt, wird das Wort "Scott" auf der `search.aspx` Seite gerendert.

#### <a name="accessing-routing-information-in-markup"></a>Zugreifen auf Routing Informationen im Markup

Die im vorherigen Abschnitt beschriebene Methode zeigt, wie Sie Routendaten in einem Code auf einer Web Forms Seite erhalten. Sie können auch Ausdrücke in Markup verwenden, mit denen Sie Zugriff auf dieselben Informationen erhalten. Ausdrucks-Generatoren sind eine leistungsstarke und elegante Möglichkeit, mit deklarativem Code zu arbeiten. (Weitere Informationen finden Sie unter dem Eintrag " [Express Yourself" mit benutzerdefinierten Ausdrucks](http://haacked.com/archive/2006/11/29/Express_Yourself_With_Custom_Expression_Builders.aspx) -Generatoren im Blog von Phil Haack.)

ASP.NET 4 enthält zwei neue Ausdrucks-Generatoren für Web Forms Routing. Im folgenden Beispiel wird gezeigt, wie Sie verwendet werden.

[!code-aspx[Main](overview/samples/sample43.aspx)]

Im Beispiel wird der *RouteUrl* -Ausdruck verwendet, um eine URL zu definieren, die auf einem Routen Parameter basiert. Dadurch ist es nicht mehr erforderlich, die gesamte URL in das Markup zu codieren, und Sie können die URL-Struktur später ändern, ohne dass eine Änderung an diesem Link erforderlich ist.

Basierend auf der zuvor definierten Route generiert dieses Markup die folgende URL:

[!code-console[Main](overview/samples/sample44.cmd)]

ASP.NET verwendet automatisch die korrekte Route (d. h., Sie generiert die richtige URL) basierend auf den Eingabe Parametern. Sie können auch einen Routennamen in den Ausdruck einschließen, mit dem Sie eine zu verwendende Route angeben können.

Im folgenden Beispiel wird gezeigt, wie der *routevalue* -Ausdruck verwendet wird.

[!code-aspx[Main](overview/samples/sample45.aspx)]

Wenn die Seite, die dieses Steuerelement enthält, ausgeführt wird, wird der Wert "Scott" in der Bezeichnung angezeigt.

Der *routevalue* -Ausdruck vereinfacht die Verwendung von Routendaten im Markup und vermeidet das Arbeiten mit der komplexeren Page. RouteData ["x"]-Syntax im Markup.

#### <a name="using-route-data-for-data-source-control-parameters"></a>Verwenden von Routendaten für Datenquellen-Steuerelement Parameter

Die *RouteParameter* -Klasse ermöglicht Ihnen das Angeben von Routendaten als Parameterwert für Abfragen in einem Datenquellen-Steuerelement. Dies [funktioniert ähnlich wie die](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formparameter.aspx) -Klasse, wie im folgenden Beispiel gezeigt:

[!code-aspx[Main](overview/samples/sample46.aspx)]

In diesem Fall wird der Wert des Routen Parameters Suchbegriff für den @companyname-Parameter in der *Select* -Anweisung verwendet.

<a id="0.2__Toc224729037"></a><a id="0.2__Toc253429261"></a><a id="0.2__Toc243304635"></a>

### <a name="setting-client-ids"></a>Festlegen von Client-IDs

Die neue *ClientIDMode* -Eigenschaft adressiert ein langjähriges Problem in ASP.net, d. wie Steuerelemente das *ID* -Attribut für Elemente erstellen, die Sie rendert. Das *ID* -Attribut für gerenderte Elemente ist wichtig, wenn die Anwendung ein Client Skript enthält, das auf diese Elemente verweist.

Das *ID* -Attribut in HTML, das für Webserver-Steuerelemente gerendert wird, wird basierend auf der *ClientID-* Eigenschaft des Steuer Elements generiert. Bis ASP.NET 4 soll der Algorithmus zum Erstellen des *ID* -Attributs aus der *ClientID-* Eigenschaft den namens Container (sofern vorhanden) mit der ID verketten und im Fall von wiederholten Steuerelementen (wie in Daten Steuerelementen) ein Präfix und eine sequenzielle Zahl hinzufügen. Obwohl dies immer gewährleistet ist, dass die IDs der Steuerelemente auf der Seite eindeutig sind, hat der Algorithmus zu Steuerelement-IDs geführt, die nicht vorhersagbar waren und daher schwer auf das Client Skript verwiesen werden konnten.

Die neue *ClientIDMode* -Eigenschaft ermöglicht es Ihnen, genauer anzugeben, wie die Client-ID für Steuerelemente generiert wird. Sie können die *ClientIDMode* -Eigenschaft für ein beliebiges Steuerelement festlegen, einschließlich für die Seite. Folgende Einstellungen sind möglich:

- *AutoID* – dies entspricht dem Algorithmus zum Generieren von *ClientID-* Eigenschafts Werten, die in früheren Versionen von ASP.NET verwendet wurden.
- *Static* – gibt an, dass der *ClientID-* Wert mit der ID übereinstimmt, ohne die IDs der übergeordneten Benennungs Container zu verketten. Dies kann in Webbenutzer Steuerelementen nützlich sein. Da sich ein Webbenutzer Steuerelement auf verschiedenen Seiten und in unterschiedlichen Container Steuerelementen befinden kann, kann es schwierig sein, Client Skripts für Steuerelemente zu schreiben, die den *AutoID* -Algorithmus verwenden, da Sie nicht vorhersagen können, was die ID-Werte sein werden.
- *Vorhersagbar* – diese Option wird in erster Linie für Daten Steuerelemente verwendet, die sich wiederholende Vorlagen verwenden. Sie verkettet die ID-Eigenschaften der Benennungs Container des Steuer Elements, aber generierte *ClientID-* Werte enthalten keine Zeichen folgen wie "ctlxxx". Diese Einstellung funktioniert in Verbindung mit der *ClientIDRowSuffix* -Eigenschaft des-Steuer Elements. Die *ClientIDRowSuffix* -Eigenschaft wird auf den Namen eines Daten Felds festgelegt, und der Wert dieses Felds wird als Suffix für den generierten *ClientID-* Wert verwendet. In der Regel verwenden Sie den Primärschlüssel eines Datensatzes als *ClientIDRowSuffix* -Wert.
- *Erben* – diese Einstellung ist das Standardverhalten für Steuerelemente. Er gibt an, dass die ID-Generierung eines Steuer Elements mit dem übergeordneten Element identisch ist.

Sie können die *ClientIDMode* -Eigenschaft auf Seitenebene festlegen. Dadurch wird der *ClientIDMode* -Standardwert für alle Steuerelemente auf der aktuellen Seite definiert.

Der Standardwert für *ClientIDMode* auf der Seitenebene lautet *AutoID*. der Standardwert *ClientIDMode* auf der Steuerungsebene ist *erben*. Wenn Sie diese Eigenschaft nicht an einer beliebigen Stelle im Code festlegen, werden alle Steuerelemente standardmäßig auf den *AutoID* -Algorithmus festgelegt.

Sie legen den Wert auf Seitenebene in der *@ Page* -Direktive fest, wie im folgenden Beispiel gezeigt:

[!code-aspx[Main](overview/samples/sample47.aspx)]

Sie können den *ClientIDMode* -Wert auch in der Konfigurationsdatei festlegen, entweder auf Computer Ebene oder auf Anwendungsebene. Dadurch wird die Standardeinstellung *ClientIDMode* für alle Steuerelemente auf allen Seiten in der Anwendung definiert. Wenn Sie den Wert auf Computer Ebene festlegen, wird die Standardeinstellung *ClientIDMode* für alle Websites auf diesem Computer definiert. Das folgende Beispiel zeigt die *ClientIDMode* -Einstellung in der Konfigurationsdatei:

[!code-xml[Main](overview/samples/sample48.xml)]

Wie bereits erwähnt, wird der Wert der *ClientID-* Eigenschaft vom Benennungs Container für das übergeordnete Element eines Steuer Elements abgeleitet. In einigen Szenarien, z. b. bei der Verwendung von Masterseiten, können Steuerelemente mit IDs wie den folgenden gerenderten HTML-Code enden:

[!code-html[Main](overview/samples/sample49.html)]

Obwohl das im Markup (aus einem *TextBox* -Steuerelement) angezeigte *Eingabe* Element nur zwei Benennungs Container auf der Seite (die verschachtelten *contentplachalter* -Steuerelemente) ist, ist das Endergebnis eine Steuerelement-ID wie die folgende:

[!code-console[Main](overview/samples/sample50.cmd)]

Diese ID ist auf der Seite garantiert eindeutig, aber für die meisten Zwecke unnötig lange. Stellen Sie sich vor, Sie möchten die Länge der gerenderten ID verringern und mehr Kontrolle darüber haben, wie die ID generiert wird. (Sie möchten z. b. "ctlxxx"-Präfixe entfernen.) Dies lässt sich am einfachsten erreichen, indem Sie die *ClientIDMode* -Eigenschaft wie im folgenden Beispiel gezeigt festlegen:

[!code-aspx[Main](overview/samples/sample51.aspx)]

In diesem Beispiel ist die *ClientIDMode* -Eigenschaft für das äußerste *namingpanel* -Element auf *static* festgelegt und für das Inner *namingcontrol* -Element auf *vorhersagbar* festgelegt. Diese Einstellungen führen zu folgendem Markup (der Rest der Seite, und die Master Seite wird als identisch mit dem im vorherigen Beispiel angesehen):

[!code-html[Main](overview/samples/sample52.html)]

Die *statische* Einstellung hat die Auswirkung, die Benennungs Hierarchie für alle Steuerelemente im äußersten *namingpanel* -Element zurückzusetzen und die *contentplachalter* -und *MasterPage* -IDs aus der generierten ID zu entfernen. (Das *Name* -Attribut der gerenderten Elemente ist nicht betroffen, sodass die normale ASP.NET-Funktionalität für Ereignisse, den Ansichts Zustand usw. beibehalten wird.) Ein Nebeneffekt beim Zurücksetzen der Benennungs Hierarchie besteht darin, dass auch dann, wenn Sie das Markup für die *namingpanel* -Elemente in ein anderes *contentplachalter* -Steuerelement verschieben, die gerenderten Client-IDs unverändert bleiben.

> [!NOTE]
> Beachten Sie, dass Sie sicherstellen müssen, dass die gerenderten Steuerelement-IDs eindeutig sind. Wenn dies nicht der Fall ist, kann die Funktion alle Funktionen unterbrechen, die eindeutige IDs für einzelne HTML-Elemente erfordern, wie z. b. die Client *Document. getElementById* -Funktion.

#### <a name="creating-predictable-client-ids-in-data-bound-controls"></a>Erstellen von vorhersagbaren Client-IDs in Daten gebundenen Steuerelementen

Die *ClientID-* Werte, die für Steuerelemente in einem Daten gebundenen Listen Steuerelement durch den Legacy Algorithmus generiert werden, können lang sein und sind nicht wirklich vorhersagbar. Mithilfe der *ClientIDMode* -Funktion können Sie besser steuern, wie diese IDs generiert werden.

Das Markup im folgenden Beispiel enthält ein *ListView* -Steuerelement:

[!code-aspx[Main](overview/samples/sample53.aspx)]

Im vorherigen Beispiel werden die *ClientIDMode* -Eigenschaft und die *rowclientidrowsuffix* -Eigenschaft im Markup festgelegt. Die *ClientIDRowSuffix* -Eigenschaft kann nur in Daten gebundenen Steuerelementen verwendet werden, und ihr Verhalten unterscheidet sich abhängig davon, welches Steuerelement Sie verwenden. Folgende Unterschiede sind zu beachten:

- *GridView* -Steuerelement – Sie können den Namen einer oder mehrerer Spalten in der Datenquelle angeben, die zur Laufzeit kombiniert werden, um die Client-IDs zu erstellen. Wenn Sie z. b. *rowclientidrowsuffix* auf "ProductName, ProductID" festlegen, haben Steuerelement-IDs für gerenderte Elemente folgendes Format:

- [!code-console[Main](overview/samples/sample54.cmd)]

- *ListView* -Steuerelement – Sie können eine einzelne Spalte in der Datenquelle angeben, die an die Client-ID angehängt wird. Wenn Sie *ClientIDRowSuffix* beispielsweise auf "ProductName" festlegen, haben die gerenderten Steuerelement-IDs ein Format wie das folgende:

- [!code-console[Main](overview/samples/sample55.cmd)]

- In diesem Fall wird der nachfolgende 1 von der Produkt-ID des aktuellen Datenelements abgeleitet.

- *Repeater* -Steuerelement – dieses Steuerelement unterstützt die *ClientIDRowSuffix* -Eigenschaft nicht. In einem *Repeater* -Steuerelement wird der Index der aktuellen Zeile verwendet. Wenn Sie ClientIDMode = "vorhersagbar" mit einem *Repeater* -Steuerelement verwenden, werden Client-IDs generiert, die das folgende Format aufweisen:

- [!code-console[Main](overview/samples/sample56.cmd)]

- Der nachfolgende 0 ist der Index der aktuellen Zeile.

Das *FormView* -Steuerelement und das *DetailsView-Steuer* Element zeigen nicht mehrere Zeilen an, sodass die *ClientIDRowSuffix* -Eigenschaft nicht unterstützt wird.

<a id="0.2__Toc224729038"></a><a id="0.2__Toc253429262"></a><a id="0.2__Toc243304636"></a>

### <a name="persisting-row-selection-in-data-controls"></a>Persistente Zeilenauswahl in Daten Steuerelementen

Mit den Steuerelementen *GridView* und *ListView* können Benutzer eine Zeile auswählen. In früheren Versionen von ASP.NET basiert die Auswahl auf dem Zeilen Index auf der Seite. Wenn Sie z. b. das dritte Element auf Seite 1 auswählen und dann auf Seite 2 verschieben, wird das dritte Element auf dieser Seite ausgewählt.

Die persistente Auswahl wurde anfänglich nur in dynamische Daten Projekten in der .NET Framework 3,5 SP1 unterstützt. Wenn dieses Feature aktiviert ist, basiert das aktuell ausgewählte Element auf dem Datenschlüssel für das Element. Dies bedeutet Folgendes: Wenn Sie die dritte Zeile auf Seite 1 auswählen und zu Seite 2 wechseln, wird auf Seite 2 nichts ausgewählt. Wenn Sie zurück zu Seite 1 wechseln, wird die dritte Zeile noch ausgewählt. Die persistente Auswahl wird jetzt für das *GridView* -Steuerelement und das *ListView* -Steuerelement in allen Projekten unterstützt, indem die *EnablePersistedSelection* -Eigenschaft verwendet wird, wie im folgenden Beispiel gezeigt:

[!code-aspx[Main](overview/samples/sample57.aspx)]

<a id="0.2__Toc253429263"></a><a id="0.2__Toc243304637"></a>

### <a name="aspnet-chart-control"></a>ASP.net-Diagramm Steuerelement

Das ASP.net- *Diagramm* Steuerelement erweitert die Daten Visualisierungs Angebote in der .NET Framework. Mithilfe des *Diagramm* Steuer Elements können Sie ASP.NET Seiten erstellen, die intuitive und visuell überzeugende Diagramme für komplexe statistische oder Finanzanalysen aufweisen. Das ASP.net- *Diagramm* Steuerelement wurde als Add-on zum .NET Framework Version 3,5 SP1 eingeführt und ist Teil des Release .NET Framework 4.

Das-Steuerelement umfasst die folgenden Features:

- 35 verschiedene Diagrammtypen
- Eine unbegrenzte Anzahl von Diagramm Flächen, Titeln, Legenden und Anmerkungen.
- Eine Vielzahl von Darstellungs Einstellungen für alle Diagramm Elemente.
- 3D-Unterstützung für die meisten Diagrammtypen.
- Intelligente Daten Bezeichnungen, die automatisch Datenpunkte anpassen können.
- Bereichs Streifen, Skalierungs Unterbrechungen und logarithmische Skalierung.
- Mehr als 50 Formeln für finanzielle und statistische Berechnungen zur Datenanalyse und -transformation
- Einfache Bindung und Bearbeitung von Diagramm Daten.
- Unterstützung für gängige Datenformate, z. b. Datumsangaben, Uhrzeiten und Währungen.
- Unterstützung für Interaktivität und ereignisgesteuerte Anpassung, einschließlich Client Click-Ereignissen mithilfe von AJAX.
- Zustandsverwaltung
- Binäres Streaming

Die folgenden Abbildungen zeigen Beispiele für Finanz Diagramme, die vom ASP.net-Diagramm Steuerelement erzeugt werden.

<a id="0.2_graphic17"></a>![](overview/_static/image1.png)

Abbildung 2: Beispiele für ASP.net-Diagramm Steuerelemente

Weitere Beispiele für die Verwendung des ASP.net Chart-Steuer Elements finden Sie auf der MSDN-Website auf der Seite [Samples Environment for Microsoft Chart Controls](https://go.microsoft.com/fwlink/?LinkId=128300) . Weitere Beispiele von Communityinhalten finden Sie im [Diagramm Steuerungs Forum](https://go.microsoft.com/fwlink/?LinkId=128713).

#### <a name="adding-the-chart-control-to-an-aspnet-page"></a>Hinzufügen des Diagramm Steuer Elements zu einer ASP.NET-Seite

Im folgenden Beispiel wird gezeigt, wie ein *Diagramm* Steuerelement mithilfe von Markup zu einer ASP.NET-Seite hinzugefügt wird. Im Beispiel erzeugt das *Diagramm* Steuerelement ein Säulendiagramm für statische Datenpunkte.

[!code-aspx[Main](overview/samples/sample58.aspx)]

#### <a name="using-3-d-charts"></a>Verwenden von 3D-Diagrammen

Das *Diagramm* Steuerelement enthält eine *ChartAreas* -Auflistung, die *ChartArea* -Objekte enthalten kann, die Merkmale von Diagramm Flächen definieren. Wenn Sie z. b. 3D für einen Diagrammbereich verwenden möchten, verwenden Sie die *Area3DStyle* -Eigenschaft wie im folgenden Beispiel:

[!code-aspx[Main](overview/samples/sample59.aspx)]

Die folgende Abbildung zeigt ein 3D-Diagramm mit vier Reihen des *Balken* Diagramm Typs.

<a id="0.2_graphic18"></a>![](overview/_static/image2.png)

Abbildung 3:3-D-Balkendiagramm

#### <a name="using-scale-breaks-and-logarithmic-scales"></a>Verwenden von Skalierungs Unterbrechungen und logarithmischen Skalen

Skalierungs Unterbrechungen und Logarithmische Skalen sind zwei zusätzliche Möglichkeiten zum Hinzufügen von Komplexität zum Diagramm. Diese Features sind spezifisch für jede Achse in einem Diagrammbereich. Wenn Sie diese Features beispielsweise auf der primären Y-Achse einer Diagrammbereich verwenden möchten, verwenden Sie die Eigenschaften *AxisY. IsLogarithmic* und *ScaleBreakStyle* in einem *ChartArea* -Objekt. Der folgende Code Ausschnitt zeigt, wie Skalierungs Unterbrechungen auf der primären Y-Achse verwendet werden.

[!code-aspx[Main](overview/samples/sample60.aspx)]

Die folgende Abbildung zeigt die Y-Achse mit aktivierten Skalierungs Unterbrechungen.

<a id="0.2_graphic19"></a>![](overview/_static/image3.png)

Abbildung 4: Skalierungs Unterbrechungen

<a id="0.2__QueryExtender"></a><a id="0.2__Toc224729041"></a><a id="0.2__Toc253429264"></a><a id="0.2__Toc243304638"></a>

### <a name="filtering-data-with-the-queryextender-control"></a>Filtern von Daten mit dem QueryExtender-Steuerelement

Eine sehr häufige Aufgabe für Entwickler, die datengesteuerte Webseiten erstellen, ist das Filtern von Daten. Dies wird normalerweise durch das aufbauen von *Where* -Klauseln in Datenquellen-Steuerelementen durchgeführt. Diese Vorgehensweise kann kompliziert sein, und in einigen Fällen kann die *Where* -Syntax nicht die vollständige Funktionalität der zugrunde liegenden Datenbank nutzen.

Um das Filtern zu vereinfachen, wurde in ASP.NET 4 ein neues *QueryExtender* -Steuerelement hinzugefügt. Dieses Steuerelement kann zu *EntityDataSource* -oder *LinqDataSource* -Steuerelementen hinzugefügt werden, um die von diesen Steuerelementen zurückgegebenen Daten zu filtern. Da das *queryextendersteuerelement* auf LINQ basiert, wird der Filter auf den Datenbankserver angewendet, bevor die Daten an die Seite gesendet werden. Dies führt zu sehr effizienten Vorgängen.

Das *QueryExtender* -Steuerelement unterstützt eine Vielzahl von Filteroptionen. In den folgenden Abschnitten werden diese Optionen beschrieben und Beispiele für deren Verwendung bereitgestellt.

#### <a name="search"></a>Suche

Für die Suchoption führt das *queryextendersteuerelement* in angegebenen Feldern eine Suche durch. Im folgenden Beispiel verwendet das-Steuerelement den Text, der im textboxsearch-Steuerelement eingegeben wird, und sucht seinen Inhalt in den `ProductName`-und `Supplier.CompanyName` Spalten in den Daten, die vom *LinqDataSource* -Steuerelement zurückgegeben werden.

[!code-aspx[Main](overview/samples/sample61.aspx)]

#### <a name="range"></a>Bereich

Die Range-Option ähnelt der Suchoption, gibt jedoch ein paar von Werten an, um den Bereich zu definieren. Im folgenden Beispiel durchsucht das *QueryExtender* -Steuerelement die `UnitPrice` Spalte in den Daten, die vom *LinqDataSource* -Steuerelement zurückgegeben werden. Der Bereich wird aus den Steuerelementen textboxfrom und textboxto auf der Seite gelesen.

[!code-aspx[Main](overview/samples/sample62.aspx)]

#### <a name="propertyexpression"></a>PropertyExpression

Mithilfe der Option Eigenschaften Ausdruck können Sie einen Vergleich mit einem Eigenschafts Wert definieren. Wenn der Ausdruck zu *true*ausgewertet wird, werden die Daten zurückgegeben, die untersucht werden. Im folgenden Beispiel filtert das *queryextendersteuerelement* Daten, indem die Daten in der `Discontinued`-Spalte mit dem Wert des checkboxnicht-Steuer Elements auf der Seite verglichen werden.

[!code-aspx[Main](overview/samples/sample63.aspx)]

#### <a name="customexpression"></a>CustomExpression

Schließlich können Sie einen benutzerdefinierten Ausdruck angeben, der mit dem *QueryExtender* -Steuerelement verwendet werden soll. Mit dieser Option können Sie eine Funktion auf der Seite aufzurufen, die benutzerdefinierte Filter Logik definiert. Im folgenden Beispiel wird gezeigt, wie ein benutzerdefinierter Ausdruck im *QueryExtender* -Steuerelement deklarativ angegeben wird.

[!code-aspx[Main](overview/samples/sample64.aspx)]

Das folgende Beispiel zeigt die benutzerdefinierte Funktion, die vom *QueryExtender* -Steuerelement aufgerufen wird. In diesem Fall verwendet der Code eine LINQ-Abfrage, um die Daten zu filtern, anstatt eine Datenbankabfrage zu verwenden, die eine *Where* -Klausel enthält.

[!code-csharp[Main](overview/samples/sample65.cs)]

In diesen Beispielen wird nur ein Ausdruck gezeigt, der im *queryextendersteuerelement* gleichzeitig verwendet wird. Sie können jedoch mehrere Ausdrücke innerhalb des *QueryExtender* -Steuer Elements einschließen.

<a id="0.2__Toc253429265"></a><a id="0.2__Toc243304639"></a>

### <a name="html-encoded-code-expressions"></a>HTML-codierte Code Ausdrücke

Einige ASP.NET Sites (insbesondere mit ASP.NET MVC) beruhen stark auf der Verwendung `<%`= `expression %>` Syntax (häufig als "Code-Nuggets" bezeichnet), um Text in die Antwort zu schreiben. Wenn Sie Code Ausdrücke verwenden, ist es einfach zu vergessen, den Text in HTML zu codieren. wenn der Text aus der Benutzereingabe stammt, kann er Seiten für einen XSS-Angriff (Cross Site Scripting) geöffnet lassen.

ASP.NET 4 führt die folgende neue Syntax für Code Ausdrücke ein:

[!code-aspx[Main](overview/samples/sample66.aspx)]

Diese Syntax verwendet beim Schreiben in die Antwort standardmäßig die HTML-Codierung. Dieser neue Ausdruck übersetzt effektiv in Folgendes:

[!code-aspx[Main](overview/samples/sample67.aspx)]

&lt;%: Request ["UserInput"]%&gt; führt z. b. die HTML-Codierung für den Wert von *"Request [" UserInput "]* durch.

Das Ziel dieses Features besteht darin, alle Instanzen der alten Syntax durch die neue Syntax zu ersetzen, damit Sie bei jedem Schritt, den Sie verwenden, nicht gezwungen werden, zu entscheiden. Es gibt jedoch Fälle, in denen der Text, der ausgegeben wird, HTML sein soll oder bereits codiert ist. in diesem Fall kann dies zur doppelten Codierung führen.

In diesen Fällen führt ASP.NET 4 eine neue Schnittstelle, *IHtmlString*, zusammen mit einer konkreten Implementierung, *HtmlString*, ein. Mit Instanzen dieser Typen können Sie angeben, dass der Rückgabewert für die Anzeige als HTML bereits ordnungsgemäß codiert (oder anderweitig untersucht) werden soll, und dass der Wert daher nicht erneut HTML-codiert werden soll. Folgendes sollte z. b. nicht (und ist nicht) HTML-codiert lauten:

[!code-aspx[Main](overview/samples/sample68.aspx)]

ASP.NET MVC 2-Hilfsmethoden wurden aktualisiert, damit Sie mit dieser neuen Syntax funktionieren, sodass Sie nicht doppelt codiert sind, sondern nur, wenn Sie ASP.NET 4 ausführen. Diese neue Syntax funktioniert nicht, wenn Sie eine Anwendung mit ASP.NET 3,5 SP1 ausführen.

Beachten Sie, dass dies keinen Schutz vor XSS-Angriffen garantiert. Beispielsweise kann HTML, das Attributwerte verwendet, die nicht in Anführungszeichen stehen, Benutzereingaben enthalten, die weiterhin anfällig sind. Beachten Sie, dass die Ausgabe der ASP.NET-Steuerelemente und ASP.NET-MVC-Hilfsprogramme immer Attributwerte in Anführungszeichen enthält. Dies ist die empfohlene Vorgehensweise.

Ebenso führt diese Syntax keine JavaScript-Codierung aus, z. b. Wenn Sie eine JavaScript-Zeichenfolge auf der Grundlage der Benutzereingabe erstellen.

<a id="0.2__Toc253429266"></a><a id="0.2__Toc243304640"></a>

### <a name="project-template-changes"></a>Änderungen an der Projektvorlage

In früheren Versionen von ASP.net, wenn Sie Visual Studio verwenden, um ein neues Website Projekt oder ein neues Webanwendungs Projekt zu erstellen, enthalten die resultierenden Projekte nur eine default. aspx-Seite, eine Standard `Web.config` Datei und den Ordner `App_Data`, wie in der folgenden Abbildung dargestellt:

<a id="0.2_graphic1A"></a>![](overview/_static/image4.png)

Visual Studio unterstützt auch einen leeren Website Projekttyp, der keine Dateien enthält, wie in der folgenden Abbildung dargestellt:

<a id="0.2_graphic1B"></a>![](overview/_static/image5.png)

Das Ergebnis ist, dass es für den Anfänger sehr wenig Anleitungen zum Erstellen einer produktionsweb Anwendung gibt. ASP.NET 4 führt daher drei neue Vorlagen ein, eine für ein leeres Webanwendungs Projekt und eine für ein Webanwendungs-und Website Projekt.

#### <a name="empty-web-application-template"></a>Leere Webanwendungs Vorlage

Wie der Name bereits vermuten lässt, ist die leere Webanwendungs Vorlage ein entblöftes Webanwendungs Projekt. Sie wählen diese Projektvorlage aus dem Visual Studio-Dialogfeld "Neues Projekt" aus, wie in der folgenden Abbildung dargestellt:

[![](overview/_static/image7.png)](overview/_static/image6.png)

([Klicken Sie, um das Bild in voller Größe anzuzeigen](overview/_static/image8.png))

Wenn Sie eine leere ASP.NET-Webanwendung erstellen, erstellt Visual Studio das folgende Ordner Layout:

<a id="0.2_graphic1D"></a>![](overview/_static/image9.png)

Dies ähnelt dem leeren Layout einer Website aus früheren Versionen von ASP.net mit einer Ausnahme. In Visual Studio 2010 enthalten leere Webanwendungen und leere Website Projekte die folgende minimale `Web.config` Datei, die Informationen enthält, die von Visual Studio verwendet werden, um das Framework zu identifizieren, für das das Projekt als Ziel verwendet wird:

<a id="0.2_graphic1E"></a>![](overview/_static/image10.png)

Ohne diese Eigenschaft *TargetFramework* verwendet Visual Studio standardmäßig den .NET Framework 2,0, um die Kompatibilität beim Öffnen älterer Anwendungen beizubehalten.

#### <a name="web-application-and-web-site-project-templates"></a>Projektvorlagen für Webanwendungen und Websites

Die anderen beiden neuen Projektvorlagen, die mit Visual Studio 2010 ausgeliefert werden, enthalten wichtige Änderungen. Die folgende Abbildung zeigt das Projekt Layout, das beim Erstellen eines neuen Webanwendungs Projekts erstellt wird. (Das Layout für ein Website Projekt ist praktisch identisch.)

- <a id="0.2_graphic1F"></a>![](overview/_static/image11.png)

Das Projekt enthält eine Reihe von Dateien, die in früheren Versionen nicht erstellt wurden. Außerdem ist das neue Webanwendungs Projekt mit grundlegenden Mitgliedschafts Funktionen konfiguriert, mit denen Sie schnell mit der Sicherung des Zugriffs auf die neue Anwendung beginnen können. Aufgrund dieser Einbindung enthält die `Web.config`-Datei für das neue Projekt Einträge, die zum Konfigurieren von Mitgliedschaft, Rollen und Profilen verwendet werden. Das folgende Beispiel zeigt die `Web.config` Datei für ein neues Webanwendungs Projekt. (In diesem Fall ist *roleManager* deaktiviert.)

[![](overview/_static/image13.png)](overview/_static/image12.png)

([Klicken Sie, um das Bild in voller Größe anzuzeigen](overview/_static/image14.png))

Das Projekt enthält außerdem eine zweite `Web.config` Datei im `Account` Verzeichnis. Die zweite Konfigurationsdatei bietet eine Möglichkeit, den Zugriff auf die ChangePassword. aspx-Seite für nicht angemeldete Benutzer zu schützen. Das folgende Beispiel zeigt den Inhalt der zweiten `Web.config` Datei.

![](overview/_static/image15.png)

Die in den neuen Projektvorlagen standardmäßig erstellten Seiten enthalten außerdem mehr Inhalt als in früheren Versionen. Das Projekt enthält eine Standard-Master Seite und eine Standard-CSS-Datei, und die Standardseite (default. aspx) ist so konfiguriert, dass die Master Seite standardmäßig verwendet wird. Dies hat zur Folge, dass beim ersten Ausführen der Webanwendung oder Website die Standardseite (Home) bereits funktionsfähig ist. Dies ist vergleichbar mit der Standardseite, die angezeigt wird, wenn Sie eine neue MVC-Anwendung starten.

[![](overview/_static/image17.png)](overview/_static/image16.png)

([Klicken Sie, um das Bild in voller Größe anzuzeigen](overview/_static/image18.png))

Die Absicht dieser Änderungen an den Projektvorlagen besteht darin, eine Anleitung zum Erstellen einer neuen Webanwendung bereitzustellen. Mit semantisch korrekten, strengen XHTML 1,0-kompatiblen Markup und mit dem mit CSS angegebenen Layout stellen die Seiten in den Vorlagen bewährte Methoden zum Erstellen von ASP.NET 4-Webanwendungen dar. Die Standardseiten verfügen auch über ein zweispaltige Layout, das Sie problemlos anpassen können.

Stellen Sie sich beispielsweise vor, dass Sie für eine neue Webanwendung einige Farben ändern und Ihr Firmenlogo anstelle des My ASP.NET-Anwendungs Logos einfügen möchten. Zu diesem Zweck erstellen Sie ein neues Verzeichnis unter `Content`, um Ihr logobild zu speichern:

<a id="0.2_graphic23"></a>![](overview/_static/image19.png)

Um das Bild der Seite hinzuzufügen, öffnen Sie die Datei `Site.Master`, suchen Sie den Speicherort der Anwendung My ASP.net, und ersetzen Sie Sie durch ein *Image* -Element, dessen *src* -Attribut auf das neue logobild festgelegt ist, wie im folgenden Beispiel gezeigt:

[![](overview/_static/image21.png)](overview/_static/image20.png)

([Klicken Sie, um das Bild in voller Größe anzuzeigen](overview/_static/image22.png))

Sie können dann in die Datei "Site. CSS" wechseln und CSS-Klassendefinitionen ändern, um sowohl die Hintergrundfarbe der Seite als auch die des Headers zu ändern.

Das Ergebnis dieser Änderungen besteht darin, dass Sie eine angepasste Startseite mit sehr geringem Aufwand anzeigen können:

[![](overview/_static/image24.png)](overview/_static/image23.png)

([Klicken Sie, um das Bild in voller Größe anzuzeigen](overview/_static/image25.png))

<a id="0.2__Toc253429267"></a><a id="0.2__Toc243304641"></a>

### <a name="css-improvements"></a>CSS-Verbesserungen

Einer der Hauptaufgaben Bereiche in ASP.NET 4 war das Rendering von HTML, das mit den neuesten HTML-Standards kompatibel ist. Dies schließt Änderungen an der Verwendung von CSS-Stilen durch ASP.NET-Webserver Steuerelemente ein.

#### <a name="compatibility-setting-for-rendering"></a>Kompatibilitäts Einstellung für Rendering

Wenn eine Webanwendung oder Website auf die .NET Framework 4 abzielt, wird das *controlRenderingCompatibilityVersion* -Attribut des *pages* -Elements standardmäßig auf "4,0" festgelegt. Dieses Element ist in der `Web.config` Datei auf Computer Ebene definiert und gilt standardmäßig für alle ASP.NET 4-Anwendungen:

[!code-xml[Main](overview/samples/sample69.xml)]

Der Wert für *controlrenderingcompatibility* ist eine Zeichenfolge, die in zukünftigen Versionen potenzielle neue Versions Definitionen zulässt. In der aktuellen Version werden die folgenden Werte für diese Eigenschaft unterstützt:

- "3.5". Diese Einstellung gibt Legacy Rendering und Markup an. Markup, das von Steuerelementen gerendert wird, ist 100% abwärts kompatibel, und die Einstellung der *xhtmlConformance* -Eigenschaft wird berücksichtigt.
- "4.0". Wenn die Eigenschaft diese Einstellung aufweist, führen ASP.NET-Webserver Steuerelemente folgende Schritte aus:
- Die *xhtmlConformance* -Eigenschaft wird immer als "Strict" behandelt. Folglich wird XHTML 1,0 Strict Markup von Steuerelementen geresbt.
- Das Deaktivieren von nicht-Eingabe Steuerelementen rendert keine ungültigen Stile mehr.
- *div* -Elemente in ausgeblendeten Feldern sind nun so formatiert, dass Sie keine Benutzer erstellten CSS-Regeln stören.
- Menü Steuerelemente Rendering-Markup, das semantisch korrekt ist und den Barrierefreiheits Richtlinien entspricht.
- Validierungs Steuerelemente werden keine Inline Stile Rendering.
- Steuerelemente, die zuvor "border =" 0 "gerendert haben (Steuerelemente, die vom ASP.net- *Tabellen* Steuerelement abgeleitet sind, und das ASP.net- *Bild* Steuerelement) Rendern dieses Attribut

#### <a name="disabling-controls"></a>Deaktivieren von Steuerelementen

In ASP.NET 3,5 SP1 und früheren Versionen rendert das Framework das *Deaktivierte* Attribut im HTML-Markup für ein beliebiges Steuerelement, dessen *aktivierte* Eigenschaft auf *false*festgelegt ist. Gemäß der HTML 4,01-Spezifikation sollten jedoch nur *Eingabe* Elemente über dieses Attribut verfügen.

In ASP.NET 4 können Sie die *controlRenderingCompatibilityVersion* -Eigenschaft auf "3,5" festlegen, wie im folgenden Beispiel gezeigt:

[!code-xml[Main](overview/samples/sample70.xml)]

Sie können Markup für ein *Label* -Steuerelement wie das folgende erstellen, das das-Steuerelement deaktiviert:

[!code-aspx[Main](overview/samples/sample71.aspx)]

Das *Label* -Steuerelement würde den folgenden HTML-Code erzeugen:

[!code-html[Main](overview/samples/sample72.html)]

In ASP.NET 4 können Sie *controlRenderingCompatibilityVersion* auf "4,0" festlegen. In diesem Fall wird nur für Steuerelemente, die *Eingabe* Elemente darstellen, ein *deaktiviertes* Attribut erstellt, wenn die *aktivierte* Eigenschaft des Steuer Elements auf *false*festgelegt ist. Steuerelemente, die keine HTML- *Eingabe* Elemente darstellen, erzeugen stattdessen ein *Klassen* Attribut, das auf eine CSS-Klasse verweist, mit der Sie ein deaktiviertes aussehen für das Steuerelement definieren können. Beispielsweise generiert das *Bezeichnung* -Steuerelement, das im vorherigen Beispiel gezeigt wurde, das folgende Markup:

[!code-html[Main](overview/samples/sample73.html)]

Der Standardwert für die-Klasse, die für dieses Steuerelement angegeben wurde, ist "aspnetdeaktiviert". Sie können diesen Standardwert jedoch ändern, indem Sie die statische *DisabledCssClass* -Eigenschaft der *WebControl* -Klasse festlegen. Für Steuerelement Entwickler kann das Verhalten, das für ein bestimmtes Steuerelement verwendet wird, auch mit der *SupportsDisabledAttribute* -Eigenschaft definiert werden.

<a id="0.2__Toc253429268"></a><a id="0.2__Toc243304642"></a>

### <a name="hiding-div-elements-around-hidden-fields"></a>Ausblenden von div-Elementen um verborgene Felder

ASP.NET 2,0 und höhere Versionen erzeugen systemspezifische verborgene Felder (z. b. das ausgeblendete Element, das zum Speichern von Ansichts Zustandsinformationen verwendet wird) innerhalb des *div* *-Elements,* um dem XHTML-Standard zu entsprechen. Dies kann jedoch zu einem Problem führen, wenn sich eine CSS-Regel auf *div* -Elemente auf einer Seite auswirkt. Dies kann z. b. dazu führen, dass eine 1-Pixel-Linie auf der Seite um verborgene *div* -Elemente angezeigt wird. In ASP.NET 4 fügen *div* -Elemente, die die von ASP.NET generierten ausgeblendeten Felder einschließen, einen CSS-Klassen Verweis hinzu, wie im folgenden Beispiel gezeigt:

[!code-html[Main](overview/samples/sample74.html)]

Anschließend können Sie eine CSS-Klasse definieren *, die nur* für die ausgeblendeten Elemente gilt, die von ASP.NET generiert werden, wie im folgenden Beispiel gezeigt:

[!code-css[Main](overview/samples/sample75.css)]

<a id="0.2__Toc253429269"></a><a id="0.2__Toc243304643"></a>

### <a name="rendering-an-outer-table-for-templated-controls"></a>Rendern einer äußeren Tabelle für Steuerelemente mit Vorlagen

Standardmäßig werden die folgenden ASP.NET-Webserver Steuerelemente, die Vorlagen unterstützen, automatisch in eine äußere Tabelle eingebunden, die zum Anwenden von Inline Stilen verwendet wird:

- *FormView*
- *Anmeldung*
- *PasswordRecovery*
- *ChangePassword*
- *Assistent*
- *CreateUserWizard*

Diesen Steuerelementen wurde eine neue Eigenschaft mit dem Namen *RenderOuterTable* hinzugefügt, mit der die äußere Tabelle aus dem Markup entfernt werden kann. Betrachten Sie z. b. das folgende Beispiel eines *FormView* -Steuer Elements:

[!code-aspx[Main](overview/samples/sample76.aspx)]

Dieses Markup rendert die folgende Ausgabe auf der Seite, die eine HTML-Tabelle enthält:

[!code-html[Main](overview/samples/sample77.html)]

Um zu verhindern, dass die Tabelle gerendert wird, können Sie die *RenderOuterTable* -Eigenschaft des *FormView* -Steuer Elements wie im folgenden Beispiel festlegen:

[!code-aspx[Main](overview/samples/sample78.aspx)]

Im vorherigen Beispiel wird die folgende Ausgabe ohne die Elemente *Table*, *TR*und *TD* gerendert:

> Inhalt

Diese Erweiterung kann das Formatieren des Inhalts des Steuer Elements mit CSS vereinfachen, da keine unerwarteten Tags durch das Steuerelement gerendert werden.

> [!NOTE]
> Beachten Sie, dass diese Änderung die Unterstützung für die Funktion zum automatischen Formatieren im Visual Studio 2010-Designer deaktiviert, da es kein *Table* -Element mehr gibt, das Stil Attribute hosten kann, die von der Option für automatisches Format generiert werden.

<a id="0.2__Toc253429270"></a><a id="0.2__Toc243304644"></a>

### <a name="listview-control-enhancements"></a>Erweiterungen des ListView-Steuer Elements

Das *ListView* -Steuerelement wurde in ASP.NET 4 einfacher zu verwenden. Die frühere Version des-Steuer Elements erforderte, dass Sie eine Layoutvorlage angeben, die ein Server Steuerelement mit einer bekannten ID enthielt. Das folgende Markup zeigt ein typisches Beispiel für die Verwendung des *ListView* -Steuer Elements in ASP.NET 3,5.

[!code-aspx[Main](overview/samples/sample79.aspx)]

In ASP.NET 4 ist für das *ListView* -Steuerelement keine Layoutvorlage erforderlich. Das im vorherigen Beispiel gezeigte Markup kann durch das folgende Markup ersetzt werden:

[!code-aspx[Main](overview/samples/sample80.aspx)]

<a id="0.2__Toc253429271"></a><a id="0.2__Toc243304645"></a>

### <a name="checkboxlist-and-radiobuttonlist-control-enhancements"></a>Verbesserungen der Check boxlist-und RadioButton List-Steuerelemente

In ASP.NET 3,5 können Sie das Layout für *CheckBoxList* und *RadioButton List* mithilfe der folgenden beiden Einstellungen angeben:

- *Flow*. Das-Steuerelement rendert *Spannen* Elemente, um seinen Inhalt zu enthalten.
- *Tabelle*. Das-Steuerelement rendert ein *Table* -Element, das seinen Inhalt enthält.

Im folgenden Beispiel wird das Markup für jedes dieser Steuerelemente angezeigt.

[!code-aspx[Main](overview/samples/sample81.aspx)]

Standardmäßig wird HTML von den Steuerelementen ähnlich wie folgt dargestellt:

[!code-html[Main](overview/samples/sample82.html)]

Da diese Steuerelemente Listen von Elementen enthalten, um semantisch korrektes HTML zu erzeugen, sollten Sie Ihren Inhalt mithilfe von HTML-Listenelementen (*Li*) Rendering. Dies erleichtert es Benutzern, die Webseiten mithilfe von Hilfstechnologien lesen, und vereinfacht das Formatieren der Steuerelemente mithilfe von CSS.

In ASP.NET 4 unterstützen die Steuerelemente *CheckBoxList* und *RadioButtonList* die folgenden neuen Werte für die *RepeatLayout* -Eigenschaft:

- *OrderedList* – der Inhalt wird als *Li* -Elemente in einem *OL* -Element gerendert.
- *UnorderedList* – der Inhalt wird in einem *UL* -Element als *Li* -Elemente gerendert.

Im folgenden Beispiel wird gezeigt, wie diese neuen Werte verwendet werden.

[!code-aspx[Main](overview/samples/sample83.aspx)]

Das vorangehende Markup generiert den folgenden HTML-Code:

[!code-html[Main](overview/samples/sample84.html)]

> [!NOTE]
> Hinweis Wenn Sie *RepeatLayout* auf *OrderedList* oder *UnorderedList*festlegen, kann die *RepeatDirection* -Eigenschaft nicht mehr verwendet werden, und es wird zur Laufzeit eine Ausnahme ausgelöst, wenn die-Eigenschaft im Markup oder Code festgelegt wurde. Die-Eigenschaft hätte keinen Wert, da das visuelle Layout dieser Steuerelemente stattdessen mithilfe von CSS definiert wird.

<a id="0.2__Toc253429272"></a><a id="0.2__Toc243304646"></a>

### <a name="menu-control-improvements"></a>Verbesserungen im Menü

Vor ASP.NET 4 hat das *Menü* Steuerelement eine Reihe von HTML-Tabellen gerendert. Dies erschwert das Anwenden von CSS-Stilen außerhalb der Einrichtung von Inline Eigenschaften und war außerdem nicht mit den Barrierefreiheits Standards kompatibel.

In ASP.NET 4 rendert das Steuerelement jetzt HTML mit semantischem Markup, das aus einer ungeordneten Liste und Listenelementen besteht. Das folgende Beispiel zeigt Markup in einer ASP.NET-Seite für das *Menü* Steuerelement.

[!code-aspx[Main](overview/samples/sample85.aspx)]

Beim Rendern der Seite erzeugt das Steuerelement den folgenden HTML-Code (der *OnClick* -Code wurde aus Gründen der Übersichtlichkeit ausgelassen):

[!code-html[Main](overview/samples/sample86.html)]

Zusätzlich zu den Renderingverbesserungen wurde die Tastaturnavigation im Menü mithilfe der Fokus Verwaltung verbessert. Wenn das *Menü* Steuerelement den Fokus erhält, können Sie die Pfeiltasten verwenden, um Elemente zu navigieren. Das *Menü* Steuerelement fügt jetzt auch barrierefreie Aria-Rollen (Rich Internet Applications) und-Attribute an,[die für den](http://www.w3.org/TR/wai-aria-practices/#menu "Leitfaden für Menü-Aria")verbesserten Zugriff zur Verfügung stehen.

Stile für das Menü Steuerelement werden in einem Stilblock am oberen Rand der Seite und nicht in der Zeile mit den gerenderten HTML-Elementen gerendert. Wenn Sie den Stil für das Steuerelement vollständig steuern möchten, können Sie die neue *IncludeStyleBlock* -Eigenschaft auf *false*festlegen. in diesem Fall wird der Stilblock nicht ausgegeben. Eine Möglichkeit, diese Eigenschaft zu verwenden, ist die Verwendung der Funktion zum automatischen Formatieren im Visual Studio-Designer, um die Darstellung des Menüs festzulegen. Sie können dann die Seite ausführen, die Seitenquelle öffnen und dann den gerenderten Stilblock in eine externe CSS-Datei kopieren. Machen Sie in Visual Studio das Format rückgängig, und legen Sie *IncludeStyleBlock* auf *false*fest. Das Ergebnis ist, dass die Menü Darstellung mithilfe von Stilen in einem externen Stylesheet definiert wird.

<a id="0.2__Toc253429273"></a><a id="0.2__Toc243304647"></a>

### <a name="wizard-and-createuserwizard-controls"></a>Assistenten und die Steuerelemente von "kreateuserwizard

Der ASP.net- *Assistent* und der " *feateuserwizard* "-Steuerelement unterstützen Vorlagen, mit denen Sie das Rendering definieren können. (*Der* Assistent wird vom *Assistenten*aus abgeleitet.) Das folgende Beispiel zeigt das Markup für ein vollständig auf Vorlagen basiertes " *kreateuserwizard* "-Steuerelement:

[!code-aspx[Main](overview/samples/sample87.aspx)]

Das-Steuerelement rendert HTML ähnlich dem folgenden:

[!code-html[Main](overview/samples/sample88.html)]

In ASP.NET 3,5 SP1, obwohl Sie den Vorlagen Inhalt ändern können, haben Sie weiterhin eingeschränkte Kontrolle über die Ausgabe des *Assistenten* -Steuer Elements. In ASP.NET 4 können Sie eine *LayoutTemplate* -Vorlage erstellen und *Platzhalter* Steuerelemente (mit reservierten Namen) einfügen, um anzugeben, wie das *Assistenten-Steuer* Element dargestellt werden soll. Dies wird im folgenden Beispiel veranschaulicht:

[!code-aspx[Main](overview/samples/sample89.aspx)]

Das Beispiel enthält die folgenden benannten Platzhalter im *LayoutTemplate* -Element:

- *headerplachalter* – zur Laufzeit wird dies durch den Inhalt des *HeaderTemplate* -Elements ersetzt.
- *sidebarplachalter* – zur Laufzeit wird dies durch den Inhalt des *SideBarTemplate* -Elements ersetzt.
- *wizardstepplachalter* – zur Laufzeit wird dies durch den Inhalt des *wizardsteptemplate* -Elements ersetzt.
- *navigationplachalter* – zur Laufzeit wird dies durch alle von Ihnen definierten Navigations Vorlagen ersetzt.

Das Markup in dem Beispiel, das Platzhalter verwendet, rendert den folgenden HTML-Code (ohne dass der Inhalt tatsächlich in den Vorlagen definiert ist):

[!code-html[Main](overview/samples/sample90.html)]

Der einzige HTML-Code, der nun Nichtbenutzer definiert ist, ist ein *Span* -Element. (Wir erwarten, dass in zukünftigen Releases auch das *Span* -Element nicht gerendert wird.) Dadurch erhalten Sie vollständige Kontrolle über praktisch alle Inhalte, die vom *Assistenten* -Steuerelement generiert werden.

<a id="0.2_dyndata"></a><a id="0.2__Toc253429274"></a><a id="0.2__Toc243304648"></a><a id="0.2__Toc224729042"></a>

## <a name="aspnet-mvc"></a>ASP.NET MVC

ASP.NET MVC wurde im März 2009 als Add-on-Framework für ASP.NET 3,5 SP1 eingeführt. Visual Studio 2010 umfasst ASP.NET MVC 2, das neue Features und Funktionen enthält.

<a id="0.2__Toc253429275"></a>

### <a name="areas-support"></a>Unterstützung von Bereichen

Bereiche ermöglichen das Gruppieren von Controllern und Ansichten in Abschnitten einer großen Anwendung in relativer Isolation von anderen Abschnitten. Jeder Bereich kann als separates ASP.NET-MVC-Projekt implementiert werden, auf das von der Hauptanwendung verwiesen werden kann. Dies hilft bei der Verwaltung von Komplexität, wenn Sie eine umfangreiche Anwendung erstellen und es für mehrere Teams einfacher ist, in einer einzigen Anwendung zusammenzuarbeiten.

<a id="0.2__Toc253429276"></a>

### <a name="data-annotation-attribute-validation-support"></a>Unterstützung der Daten Annotation-Attribut Validierung

Mit *DataAnnotations* -Attributen können Sie mithilfe von Metadatenattributen Validierungs Logik an ein Modell anfügen. *DataAnnotations* -Attribute wurden in ASP.net dynamische Daten in ASP.NET 3,5 SP1 eingeführt. Diese Attribute wurden in den Standardmodell Binder integriert und stellen eine metadatengesteuerte Möglichkeit zum Überprüfen von Benutzereingaben bereit.

<a id="0.2__Toc253429277"></a>

### <a name="templated-helpers"></a>Auf Vorlagen basierende Hilfsprogramme

Mit Vorlagen basierten Hilfsprogrammen können Sie die Bearbeitungs-und Anzeige Vorlagen automatisch den-Datentypen zuordnen. Beispielsweise können Sie mithilfe einer Vorlagen Hilfe angeben, dass ein Benutzeroberflächen Element für die Datumsauswahl automatisch für einen *System. DateTime* -Wert gerendert wird. Dies ähnelt den Feld Vorlagen in ASP.net-dynamische Daten.

Die Hilfsmethoden *HTML. EditorFor* und *HTML. DisplayFor* verfügen über integrierte Unterstützung für das Rendern von Standard Datentypen sowie für komplexe Objekte mit mehreren Eigenschaften. Sie passen auch das Rendering an, indem Sie Daten Annotation-Attribute wie *Display Name* und *Gerüst Column* auf das *ViewModel* -Objekt anwenden können.

Häufig ist es ratsam, die Ausgabe der Benutzeroberflächen-Hilfsprogramme noch weiter anzupassen und eine umfassende Kontrolle darüber zu haben, was generiert wird. Die Hilfsmethoden *HTML. EditorFor* und *HTML. DisplayFor* unterstützen dies mithilfe eines Vorlagen Mechanismus, mit dem Sie externe Vorlagen definieren können, die die gerenderte Ausgabe überschreiben und steuern können. Die Vorlagen können einzeln für eine Klasse gerendert werden.

<a id="0.2__Toc253429278"></a><a id="0.2__Toc243304649"></a>

## <a name="dynamic-data"></a>Dynamic Data

Dynamische Daten wurde in der .NET Framework 3,5 SP1-Version Mitte 2008 eingeführt. Diese Funktion bietet viele Erweiterungen für das Erstellen von datengesteuerten Anwendungen, einschließlich der folgenden:

- Ein Rad-Prozess für das schnelle Entwickeln einer datengesteuerten Website.
- Automatische Validierung, die auf Einschränkungen basiert, die im Datenmodell definiert sind.
- Die Möglichkeit, das Markup, das für Felder in den *GridView* -und *DetailsView* -Steuerelementen generiert wird, mithilfe von Feld Vorlagen, die Teil des dynamische Daten Projekts sind, leicht zu ändern.

> [!NOTE]
> Hinweis Weitere Informationen finden Sie in der [dynamische Daten-Dokumentation](https://msdn.microsoft.com/library/cc488545.aspx) in der MSDN Library.

Für ASP.NET 4 wurde dynamische Daten verbessert, um Entwicklern eine noch bessere Leistung für das schnelle entwickeln Daten gesteuerter Websites zu verschaffen.

<a id="0.2__Toc253429279"></a><a id="0.2__Toc243304650"></a>

### <a name="enabling-dynamic-data-for-existing-projects"></a>Aktivieren von dynamische Daten für vorhandene Projekte

Dynamische Daten Features, die im .NET Framework 3,5 SP1 ausgeliefert wurden, haben neue Features wie die folgenden eingeführt:

- Feld Vorlagen – diese Stellen Datentyp basierte Vorlagen für Daten gebundene Steuerelemente bereit. Feld Vorlagen stellen eine einfachere Möglichkeit dar, das Aussehen von Daten Steuerelementen anzupassen, als Vorlagen Felder für jedes Feld zu verwenden.
- Validierung – dynamische Daten ermöglicht Ihnen das Verwenden von Attributen für Daten Klassen, um die Validierung für gängige Szenarien anzugeben, wie z. b. erforderliche Felder, Bereichs Überprüfung, Typüberprüfung, Muster Vergleich mit regulären Ausdrücken und benutzerdefinierte Validierung. Die Validierung wird durch die Daten Steuerelemente erzwungen.

Für diese Features gelten jedoch die folgenden Anforderungen:

- Die Datenzugriffs Ebene musste auf Entity Framework oder LINQ to SQL basieren.
- Die einzigen Datenquellen Steuerelemente, die für diese Features unterstützt werden, waren die *EntityDataSource* -oder *LinqDataSource* -Steuerelemente.
- Die Funktionen erforderten ein Webprojekt, das mit den Vorlagen für dynamische Daten oder dynamische Daten Entitäten erstellt wurde, um alle Dateien zu verwenden, die zur Unterstützung der Funktion erforderlich waren.

Ein wichtiges Ziel der dynamische Daten Unterstützung in ASP.NET 4 besteht darin, die neue Funktionalität von dynamische Daten für jede ASP.NET-Anwendung zu aktivieren. Im folgenden Beispiel wird das Markup für Steuerelemente gezeigt, die dynamische Daten Funktionen in einer vorhandenen Seite nutzen können.

[!code-aspx[Main](overview/samples/sample91.aspx)]

Im Code für die Seite muss folgender Code hinzugefügt werden, um dynamische Daten Unterstützung für diese Steuerelemente zu aktivieren:

[!code-csharp[Main](overview/samples/sample92.cs)]

Wenn sich das *GridView* -Steuerelement im Bearbeitungsmodus befindet, überprüft dynamische Daten automatisch, ob die eingegebenen Daten im richtigen Format vorliegen. Wenn dies nicht der Fall ist, wird eine Fehlermeldung angezeigt.

Diese Funktionalität bietet auch andere Vorteile, z. b. das Festlegen von Standardwerten für den Einfügemodus. Ohne dynamische Daten müssen Sie einen Standardwert für ein Feld implementieren, indem Sie an ein Ereignis anfügen, das Steuerelement (mithilfe von *FindControl*) suchen und dessen Wert festlegen. In ASP.NET 4 unterstützt der *EnableDynamicData* -Befehl einen zweiten Parameter, mit dem Sie Standardwerte für jedes Feld des Objekts übergeben können, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](overview/samples/sample93.cs)]

<a id="0.2__Toc224729043"></a><a id="0.2__Toc253429280"></a><a id="0.2__Toc243304651"></a>

### <a name="declarative-dynamicdatamanager-control-syntax"></a>Deklarative DynamicDataManager-Steuerelement Syntax

Das *DynamicDataManager* -Steuerelement wurde verbessert, sodass Sie es deklarativ konfigurieren können, wie bei den meisten Steuerelementen in ASP.net, anstatt nur im Code. Das Markup für das *DynamicDataManager* -Steuerelement sieht wie im folgenden Beispiel aus:

[!code-aspx[Main](overview/samples/sample94.aspx)]

Dieses Markup ermöglicht dynamische Daten Verhalten für das GridView1-Steuerelement, auf das im *DataControls* -Abschnitt des *DynamicDataManager* -Steuer Elements verwiesen wird.

<a id="0.2__Toc224729044"></a><a id="0.2__Toc253429281"></a><a id="0.2__Toc243304652"></a>

### <a name="entity-templates"></a>Entitäts Vorlagen

Entitäts Vorlagen bieten eine neue Möglichkeit zum Anpassen des Layouts von Daten, ohne dass Sie eine benutzerdefinierte Seite erstellen müssen. Seitenvorlagen verwenden das *FormView* -Steuerelement (anstelle des *DetailsView* -Steuer Elements, wie in Seitenvorlagen in früheren Versionen von dynamische Daten verwendet) und das *dynamizentity* -Steuerelement zum Rendering von Entitäts Vorlagen. Dadurch erhalten Sie mehr Kontrolle über das Markup, das von dynamische Daten gerendert wird.

Die folgende Liste zeigt das neue Projektverzeichnis Layout, das Entitäts Vorlagen enthält:

[!code-console[Main](overview/samples/sample95.cmd)]

Das `EntityTemplate`-Verzeichnis enthält Vorlagen zum Anzeigen von Datenmodell Objekten. Standardmäßig werden-Objekte mithilfe der `Default.ascx`-Vorlage gerendert, die das Markup enthält, das wie das von dem *DetailsView* -Steuerelement, das von dynamische Daten in ASP.NET 3,5 SP1 verwendet wird, erstellte Markup aussieht. Das folgende Beispiel zeigt das Markup für das `Default.ascx`-Steuerelement:

[!code-aspx[Main](overview/samples/sample96.aspx)]

Die Standardvorlagen können bearbeitet werden, um das Erscheinungsbild für den gesamten Standort zu ändern. Es sind Vorlagen für Anzeige-, Bearbeitungs-und Einfügevorgänge vorhanden. Basierend auf dem Namen des Datenobjekts können neue Vorlagen hinzugefügt werden, um das Erscheinungsbild von nur einem Objekttyp zu ändern. Beispielsweise können Sie die folgende Vorlage hinzufügen:

[!code-console[Main](overview/samples/sample97.cmd)]

Die Vorlage enthält möglicherweise das folgende Markup:

[!code-aspx[Main](overview/samples/sample98.aspx)]

Die neuen Entitäts Vorlagen werden mithilfe des neuen *dynamicenti-* Steuer Elements auf einer Seite angezeigt. Zur Laufzeit wird dieses Steuerelement durch den Inhalt der Entitäts Vorlage ersetzt. Das folgende Markup zeigt das *FormView* -Steuerelement in der `Detail.aspx` Seitenvorlage, die die Entitäts Vorlage verwendet. Beachten Sie das *DynamicEntity* -Element im Markup.

[!code-aspx[Main](overview/samples/sample99.aspx)]

<a id="0.2__Toc224729045"></a><a id="0.2__Toc253429282"></a><a id="0.2__Toc243304653"></a>

### <a name="new-field-templates-for-urls-and-email-addresses"></a>Neue Feld Vorlagen für URLs und e-Mail-Adressen

ASP.NET 4 führt zwei neue integrierte Feld Vorlagen ein, `EmailAddress.ascx` und `Url.ascx`. Diese Vorlagen werden für Felder verwendet, die als *EmailAddress* oder *URL* mit dem *DataType* -Attribut gekennzeichnet sind. Für *EmailAddress* -Objekte wird das Feld als Link angezeigt, der mit dem *mailto:* -Protokoll erstellt wird. Wenn Benutzer auf den Link klicken, wird der e-Mail-Client des Benutzers geöffnet, und es wird eine Skeleton-Meldung erstellt. Als *URL* typisierte Objekte werden als normale Hyperlinks angezeigt.

Im folgenden Beispiel wird gezeigt, wie Felder markiert werden.

[!code-csharp[Main](overview/samples/sample100.cs)]

<a id="0.2__Toc224729046"></a><a id="0.2__Toc253429283"></a><a id="0.2__Toc243304654"></a>

### <a name="creating-links-with-the-dynamichyperlink-control"></a>Erstellen von Verknüpfungen mit dem DynamicHyperLink-Steuerelement

Dynamische Daten verwendet das neue Routing Feature, das in der .NET Framework 3,5 SP1 hinzugefügt wurde, um die URLs zu steuern, die Endbenutzern beim Zugriff auf die Website angezeigt werden. Mit dem neuen *DynamicHyperLink* -Steuerelement können Sie ganz einfach Links zu Seiten auf einer dynamische Daten Site erstellen. Im folgenden Beispiel wird gezeigt, wie das *DynamicHyperLink* -Steuerelement verwendet wird:

[!code-aspx[Main](overview/samples/sample101.aspx)]

Dieses Markup erstellt einen Link, der auf die Listenseite für die `Products` Tabelle basierend auf Routen verweist, die in der `Global.asax`-Datei definiert sind. Das-Steuerelement verwendet automatisch den Standard Tabellennamen, auf dem die dynamische Daten Seite basiert.

<a id="0.2__Toc224729047"></a><a id="0.2__Toc253429284"></a><a id="0.2__Toc243304655"></a>

### <a name="support-for-inheritance-in-the-data-model"></a>Unterstützung für Vererbung im Datenmodell

Sowohl der Entity Framework als auch LINQ to SQL unterstützen die Vererbung in ihren Datenmodellen. Ein Beispiel hierfür ist eine Datenbank, die über eine `InsurancePolicy` Tabelle verfügt. Sie kann auch `CarPolicy` und `HousePolicy` Tabellen enthalten, die dieselben Felder wie `InsurancePolicy` aufweisen, und dann weitere Felder hinzufügen. Dynamische Daten wurde geändert, um geerbte Objekte im Datenmodell zu verstehen und Gerüstbau für die geerbten Tabellen zu unterstützen.

<a id="0.2__Toc224729048"></a><a id="0.2__Toc253429285"></a><a id="0.2__Toc243304656"></a>

### <a name="support-for-many-to-many-relationships-entity-framework-only"></a>Unterstützung für m:n-Beziehungen (nur Entity Framework)

Der Entity Framework verfügt über eine umfassende Unterstützung für m:n-Beziehungen zwischen Tabellen, die implementiert wird, indem die Beziehung als Auflistung für ein *Entitäts* Objekt verfügbar gemacht wird. Neue `ManyToMany.ascx` und `ManyToMany_Edit.ascx` Feld Vorlagen wurden hinzugefügt, um die Anzeige und Bearbeitung von Daten zu unterstützen, die an m:n-Beziehungen beteiligt sind.

<a id="0.2__Toc224729049"></a><a id="0.2__Toc253429286"></a><a id="0.2__Toc243304657"></a>

### <a name="new-attributes-to-control-display-and-support-enumerations"></a>Neue Attribute zum Steuern der Anzeige und Unterstützung von Enumerationen

Das *DisplayAttribute-Attribut* wurde hinzugefügt, um Ihnen eine zusätzliche Kontrolle über die Anzeige von Feldern zu verschaffen. Das *Display Name* -Attribut in früheren Versionen von dynamische Daten es Ihnen ermöglicht, den Namen zu ändern, der als Beschriftung für ein Feld verwendet wird. Mit der neuen *DisplayAttribute* -Klasse können Sie weitere Optionen zum Anzeigen eines Felds angeben, z. b. die Reihenfolge, in der ein Feld angezeigt wird, und ob ein Feld als Filter verwendet wird. Das Attribut bietet auch eine unabhängige Kontrolle über den Namen, der für die Bezeichnungen in einem *GridView* -Steuerelement verwendet wird, den Namen, der in einem *DetailsView* -Steuerelement verwendet wird, den Hilfetext für das Feld und das Wasserzeichen, das für das Feld verwendet wird (wenn das Feld Texteingaben annimmt).

Die *enumdatatypeattribute* -Klasse wurde hinzugefügt, um das Zuordnen von Feldern zu Enumerationen zu ermöglichen. Wenn Sie dieses Attribut auf ein Feld anwenden, geben Sie einen Enumerationstyp an. Dynamische Daten verwendet die neue `Enumeration.ascx` Feld Vorlage zum Erstellen einer Benutzeroberfläche zum Anzeigen und Bearbeiten von Enumerationswerten. Die Vorlage ordnet die Werte aus der-Datenbank den Namen in der-Enumeration zu.

<a id="0.2__Toc224729050"></a><a id="0.2__Toc253429287"></a><a id="0.2__Toc243304658"></a>

### <a name="enhanced-support-for-filters"></a>Erweiterte Unterstützung für Filter

Dynamische Daten 1,0 wurde mit integrierten Filtern für boolesche Spalten und Fremdschlüssel Spalten ausgeliefert. In den Filtern konnten Sie nicht angeben, ob Sie angezeigt wurden oder in welcher Reihenfolge Sie angezeigt wurden. Das neue *DisplayAttribute* -Attribut behandelt beide Probleme, indem Sie steuern, ob eine Spalte als Filter angezeigt wird und in welcher Reihenfolge Sie angezeigt wird.

Eine weitere Erweiterung besteht darin, dass die Filter Unterstützung neu[geschrieben wurde, um die neue](#0.2__QueryExtender "_QueryExtender") Funktion von Web Forms zu verwenden. Dies ermöglicht es Ihnen, Filter zu erstellen, ohne dass Kenntnisse über das Datenquellen-Steuerelement erforderlich sind, mit dem die Filter verwendet werden. Neben diesen Erweiterungen wurden auch Filter in Vorlagen Steuerelemente umgewandelt, sodass Sie neue hinzufügen können. Schließlich ermöglicht die zuvor erwähnte *DisplayAttribute* -Klasse, dass der Standardfilter überschrieben wird, und zwar auf dieselbe Weise, wie *UIHint* zulässt, dass die Standardfeld Vorlage für eine Spalte überschrieben wird.

<a id="0.2__Toc224729051"></a><a id="0.2__Toc253429288"></a><a id="0.2__Toc243304659"></a>

## <a name="visual-studio-2010-web-development-improvements"></a>Verbesserungen der Webentwicklung in Visual Studio 2010

Die Webentwicklung in Visual Studio 2010 wurde verbessert, um eine höhere CSS-Kompatibilität, eine höhere Produktivität durch HTML-und ASP.NET-Markup Ausschnitte und ein neues dynamisches IntelliSense-JavaScript zu erhalten.

<a id="0.2__Toc224729052"></a><a id="0.2__Toc253429289"></a><a id="0.2__Toc243304660"></a>

### <a name="improved-css-compatibility"></a>Verbesserte CSS-Kompatibilität

Der Visual Web Developer-Designer in Visual Studio 2010 wurde aktualisiert, um die Kompatibilität der CSS 2,1-Standards zu verbessern. Der Designer behält die Integrität der HTML-Quelle besser bei und ist robuster als in früheren Versionen von Visual Studio. Im Hintergrund wurden auch architektonische Verbesserungen vorgenommen, die zukünftige Verbesserungen bei Rendering, Layout und Serviceability verbessern.

<a id="0.2__Toc224729053"></a><a id="0.2__Toc253429290"></a><a id="0.2__Toc243304661"></a>

### <a name="html-and-javascript-snippets"></a>HTML-und JavaScript-Code Ausschnitte

Im HTML-Editor vervollständigt IntelliSense Tagnamen automatisch. Das Feature IntelliSense-Ausschnitte schließt automatisch vollständige Tags und mehr ab. In Visual Studio 2010 werden IntelliSense-Ausschnitte neben C# und Visual Basic für JavaScript unterstützt, die in früheren Versionen von Visual Studio unterstützt wurden.

Visual Studio 2010 umfasst mehr als 200 Ausschnitte, mit denen Sie gängige ASP.net-und HTML-Tags automatisch vervollständigen können, einschließlich erforderlicher Attribute (z. b. runat = "Server") und allgemeiner Attribute, die für ein Tag spezifisch sind (z. b. *ID*, *DataSourceID*, *ControlToValidate*und *Text*).

Sie können zusätzliche Code Ausschnitte herunterladen, oder Sie können eigene Ausschnitte schreiben, die die von Ihnen oder Ihrem Team für häufige Aufgaben verwendeten Markup Blöcke Kapseln.

<a id="0.2__Toc224729054"></a><a id="0.2__Toc253429291"></a><a id="0.2__Toc243304662"></a>

### <a name="javascript-intellisense-enhancements"></a>JavaScript-IntelliSense-Erweiterungen

In Visual 2010 wurde JavaScript IntelliSense neu gestaltet, um eine noch umfassendere Bearbeitungsfunktion bereitzustellen. IntelliSense erkennt jetzt Objekte, die durch Methoden wie *Register Namespace* und durch ähnliche Techniken, die von anderen JavaScript-Frameworks verwendet werden, dynamisch generiert wurden. Die Leistung wurde verbessert, um große Skript Skripts zu analysieren und IntelliSense mit wenigen oder gar keinen Verarbeitungs Verzögerungen anzuzeigen. Die Kompatibilität wurde erheblich erhöht, sodass fast alle Bibliotheken von Drittanbietern unterstützt werden und verschiedene Codierungs Stile unterstützt werden. Dokumentations Kommentare werden nun während der typverarbeitung analysiert und sofort von IntelliSense genutzt.

<a id="0.2__Toc224729055"></a><a id="0.2__Toc253429292"></a><a id="0.2__Toc243304663"></a>

## <a name="web-application-deployment-with-visual-studio-2010"></a>Webanwendungs Bereitstellung mit Visual Studio 2010

Wenn ASP.NET-Entwickler eine Webanwendung bereitstellen, stellen Sie häufig fest, dass folgende Probleme auftreten:

- Für die Bereitstellung auf einer freigegebenen Hostingsite sind Technologien wie FTP erforderlich, was langsam sein kann. Außerdem müssen Sie manuell Aufgaben ausführen, z. b. das Ausführen von SQL-Skripts zum Konfigurieren einer Datenbank, und Sie müssen die IIS-Einstellungen ändern, z. b. das Konfigurieren eines virtuellen Verzeichnis Ordners als Anwendung.
- In einer Unternehmensumgebung müssen Administratoren neben der Bereitstellung der Webanwendungs Dateien häufig ASP.NET-Konfigurationsdateien und IIS-Einstellungen ändern. Datenbankadministratoren müssen eine Reihe von SQL-Skripts ausführen, um die Ausführung der Anwendungsdatenbank zu erreichen. Solche Installationen sind arbeitsintensiv, nehmen oft Stunden in Anspruch und müssen sorgfältig dokumentiert werden.

Visual Studio 2010 umfasst Technologien, mit denen diese Probleme behoben werden und die eine nahtlose Bereitstellung von Webanwendungen ermöglichen. Eine dieser Technologien ist das IIS-Webbereitstellungs Tool (msdeployment. exe).

Zu den Webbereitstellungs Funktionen in Visual Studio 2010 gehören die folgenden Hauptbereiche:

- Webverpackung
- Web. config-Transformation
- Daten Bank Bereitstellung
- One-Click-Veröffentlichung für Webanwendungen

In den folgenden Abschnitten finden Sie Details zu diesen Features.

<a id="0.2__Toc224729056"></a><a id="0.2__Toc253429293"></a><a id="0.2__Toc243304664"></a>

### <a name="web-packaging"></a>Webverpackung

Visual Studio 2010 verwendet das Tool msbereitstellung zum Erstellen einer komprimierten Datei (ZIP-Datei) für Ihre Anwendung, die als *Webpaket*bezeichnet wird. Die Paketdatei enthält Metadaten zu Ihrer Anwendung sowie den folgenden Inhalt:

- IIS-Einstellungen, einschließlich Einstellungen für den Anwendungs Pool, Einstellungen für Fehlerseiten usw.
- Der tatsächliche Webinhalt, der Webseiten, Benutzer Steuerelemente, statische Inhalte (Bilder und HTML-Dateien) usw. enthält.
- SQL Server Datenbankschemas und Daten.
- Sicherheitszertifikate, Komponenten, die im GAC installiert werden sollen, Registrierungs Einstellungen usw.

Ein Webpaket kann auf einen beliebigen Server kopiert und dann manuell mithilfe des IIS-Managers installiert werden. Alternativ kann das Paket für die automatisierte Bereitstellung mithilfe von Befehlszeilen Befehlen oder mithilfe von Bereitstellungs-APIs installiert werden.

Visual Studio 2010 stellt integrierte MSBuild-Aufgaben und-Ziele zum Erstellen von Webpaketen bereit. Weitere Informationen finden Sie unter [Übersicht über die Projekt Bereitstellung ASP.NET Webanwendungen](https://msdn.microsoft.com/library/dd394698%28VS.100%29.aspx) auf der MSDN-Website und aus [10 + 20 Gründen, warum Sie ein Webpaket](http://vishaljoshi.blogspot.com/2009/07/10-20-reasons-why-you-should-create-web.html) im Blog von Vishal Joshi erstellen sollten.

<a id="0.2__Toc224729057"></a><a id="0.2__Toc253429294"></a><a id="0.2__Toc243304665"></a>

### <a name="webconfig-transformation"></a>Web. config-Transformation

Bei der Bereitstellung von Webanwendungen führt Visual Studio 2010 eine [XML Document Transform (xdt)](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html)ein, bei der es sich um eine Funktion handelt, mit der Sie eine `Web.config` Datei von Entwicklungseinstellungen in Produktionseinstellungen transformieren können. Transformations Einstellungen werden in Transformations Dateien namens "`web.debug.config`", "`web.release.config`" und so weiter angegeben. (Die Namen dieser Dateien entsprechen den MSBuild-Konfigurationen.) Eine Transformations Datei enthält nur die Änderungen, die Sie an einer bereitgestellten `Web.config` Datei vornehmen müssen. Sie geben die Änderungen mithilfe der einfachen Syntax an.

Das folgende Beispiel zeigt einen Teil einer `web.release.config`-Datei, die möglicherweise für die Bereitstellung der Releasekonfiguration erstellt wird. Das Replace-Schlüsselwort im Beispiel gibt an, dass der *ConnectionString* -Knoten in der `Web.config`-Datei während der Bereitstellung durch die Werte ersetzt wird, die im Beispiel aufgeführt sind.

[!code-xml[Main](overview/samples/sample102.xml)]

Weitere Informationen finden Sie unter [Web. config-Transformations Syntax für die Webanwendungs Projekt Bereitstellung](https://msdn.microsoft.com/library/dd465326%28VS.100%29.aspx) auf der MSDN <a id="0.2_a"></a> -Website und in der[Webbereitstellung: Web. config-Transformation](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html) im Blog von Vishal Joshi.

<a id="0.2__Toc224729058"></a><a id="0.2__Toc253429295"></a><a id="0.2__Toc243304666"></a>

### <a name="database-deployment"></a>Daten Bank Bereitstellung

Ein Visual Studio 2010-Bereitstellungs Paket kann Abhängigkeiten von SQL Server Datenbanken enthalten. Als Teil der Paket Definition geben Sie die Verbindungs Zeichenfolge für die Quelldatenbank an. Wenn Sie das Webpaket erstellen, erstellt Visual Studio 2010 SQL-Skripts für das Datenbankschema und optional für die Daten und fügt diese dann zum Paket hinzu. Sie können auch benutzerdefinierte SQL-Skripts bereitstellen und die Reihenfolge angeben, in der Sie auf dem Server ausgeführt werden sollen. Zum Zeitpunkt der Bereitstellung stellen Sie eine Verbindungs Zeichenfolge bereit, die für den Zielserver geeignet ist. der Bereitstellungs Prozess verwendet dann diese Verbindungs Zeichenfolge, um die Skripts auszuführen, die das Datenbankschema erstellen und die Daten hinzufügen.

Darüber hinaus können Sie mithilfe der One-Click-Veröffentlichung die Bereitstellung so konfigurieren, dass die Datenbank direkt veröffentlicht wird, wenn die Anwendung auf einem freigegebenen Remote hostingstandort veröffentlicht wird. Weitere Informationen finden Sie unter Gewusst [wie: Bereitstellen einer Datenbank mit einem Webanwendungs Projekt](https://msdn.microsoft.com/library/dd465343%28VS.100%29.aspx) auf der MSDN-Website und [Daten Bank Bereitstellung mit VS 2010](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html) im Blog von Vishal Joshi.

<a id="0.2__Toc224729059"></a><a id="0.2__Toc253429296"></a><a id="0.2__Toc243304667"></a>

### <a name="one-click-publish-for-web-applications"></a>One-Click-Veröffentlichung für Webanwendungen

Mit Visual Studio 2010 können Sie auch den IIS-Remote Verwaltungsdienst zum Veröffentlichen einer Webanwendung auf einem Remote Server verwenden. Sie können ein Veröffentlichungs Profil für Ihr Hostingkonto oder für Testserver oder Stagingserver erstellen. In jedem Profil können geeignete Anmelde Informationen sicher gespeichert werden. Sie können dann auf einem der Zielserver mit einem Mausklick bereitstellen, indem Sie die Web-One-Click-Veröffentlichungs Symbolleiste verwenden. Mit Visual Studio 2010 können Sie auch mithilfe der MSBuild-Befehlszeile veröffentlichen. Auf diese Weise können Sie Ihre teambuildumgebung so konfigurieren, dass Sie die Veröffentlichung in einem Modell mit kontinuierlicher Integration einschließt.

Weitere Informationen finden Sie unter Gewusst [wie: Bereitstellen eines Webanwendungs Projekts mit One-Click-Veröffentlichung und-Web deploy](https://msdn.microsoft.com/library/dd465337%28VS.100%29.aspx) auf der MSDN-Website und [Web 1: Klicken Sie](http://vishaljoshi.blogspot.com/2009/05/web-1-click-publish-with-vs-2010.html) im Blog von Vishal Joshi auf Veröffentlichen mit VS 2010. Videopräsentationen zur Bereitstellung von Webanwendungen in Visual Studio 2010 finden Sie unter [vs 2010 for Web Developer Previews](http://vishaljoshi.blogspot.com/2008/12/vs-2010-for-web-developer-previews.html) im Blog von Vishal Joshi.

<a id="0.2__Toc224729060"></a><a id="0.2__Toc253429297"></a><a id="0.2__Toc243304668"></a>

### <a name="resources"></a>Ressourcen

Die folgenden Websites bieten zusätzliche Informationen zu ASP.NET 4 und Visual Studio 2010.

- [ASP.NET 4](https://msdn.microsoft.com/library/ee532866%28VS.100%29.aspx) – die offizielle Dokumentation für ASP.NET 4 auf der MSDN-Website.
- [https://www.asp.net/](https://www.asp.net/) – die eigene Website des ASP.NET-Teams.
- [https://www.asp.net/dynamicdata/](https://msdn.microsoft.com/library/cc488545.aspx) und [ASP.net dynamische Daten Inhalts](https://msdn.microsoft.com/library/cc488545%28VS.100%29.aspx) Zuordnung – Online-Ressourcen auf der ASP.net-Teamwebsite und in der offiziellen Dokumentation für ASP.net dynamische Daten.
- [https://www.asp.net/ajax/](../../ajax/index.md) – die zentrale Webressource für die ASP.NET AJAX-Entwicklung.
- [https://blogs.msdn.com/webdevtools/](https://blogs.msdn.com/webdevtools/) – der Blog des Visual Web Developer-Teams, der Informationen zu Funktionen in Visual Studio 2010 enthält.
- [ASP.net webstack](https://github.com/aspnet/AspNetWebStack) – die zentrale Webressource für Vorschau Versionen von ASP.net.

<a id="0.2__Toc224729061"></a><a id="0.2__Toc253429298"></a><a id="0.2__Toc243304669"></a>

## <a name="disclaimer"></a>Haftungsausschluss

Dies ist ein vorläufiges Dokument, das vor der kommerziellen Veröffentlichung der beschriebenen Software ggf. erheblich geändert wird.

Die in diesem Dokument enthaltenen Informationen stellen die Sicht der Microsoft Corporation der hier diskutierten Themen zum Zeitpunkt der Veröffentlichung dar. Da Microsoft auf wechselnde Marktbedingungen reagieren muss, sollten sie nicht als Verpflichtung seitens Microsoft interpretiert werden, und Microsoft kann die Genauigkeit der dargelegten Informationen nach dem Zeitpunkt der Veröffentlichung nicht garantieren.

Dieses Whitepaper dient ausschließlich Informationszwecken. MICROSOFT ÜBERNIMMT KEINE AUSDRÜCKLICHE, STILLSCHWEIGENDE ODER AUS GESETZ ERWACHSENDE GARANTIE IN BEZUG AUF DIE INFORMATIONEN IN DIESEM DOKUMENT.

Die Benutzer/innen sind verpflichtet, sich an alle anwendbaren Urheberrechtsgesetze zu halten. Unabhängig von der Anwendbarkeit der entsprechenden Urheberrechtsgesetze darf kein Teil dieses Dokuments ohne ausdrückliche schriftliche Erlaubnis der Microsoft Corporation für irgendwelche Zwecke vervielfältigt oder in einem Datenempfangssystem gespeichert oder darin eingelesen werden, unabhängig davon, auf welche Art und Weise oder mit welchen Mitteln (elektronisch, mechanisch, durch Fotokopieren, Aufzeichnen usw.) dies geschieht.

Microsoft Corporation kann Inhaber von Patenten oder Patentanträgen, Marken, Urheberrechten oder anderem geistigen Eigentum sein, die den Inhalt dieses Dokuments betreffen. Die Bereitstellung dieses Dokuments gewährt keinerlei Lizenzrechte an diesen Patenten, Marken, Urheberrechten oder anderem geistigen Eigentum, es sei denn, dies wurde ausdrücklich durch einen schriftlichen Lizenzvertrag mit der Microsoft Corporation vereinbart.

Sofern nicht anders angegeben, sind die hier dargestellten Beispiel Unternehmen, Organisationen, Produkte, Domänen Namen, e-Mail-Adressen, Logos, Personen, Orte und Ereignisse fiktiv und keine Zuordnung zu echten Unternehmen, Organisationen, Produkten, Domänen Namen, e-Mail-Adressen. Adresse, Logo, Person, Ort oder Ereignis ist beabsichtigt oder sollte abgeleitet werden.

© 2009 Microsoft Corporation. Alle Rechte vorbehalten.

Microsoft und Windows sind eingetragene Marken oder Marken der Microsoft Corporation in den USA und/oder anderen Ländern.

Die in diesem Dokument erwähnten Namen von Unternehmen und Produkten können Marken der jeweiligen Eigentümer sein.
