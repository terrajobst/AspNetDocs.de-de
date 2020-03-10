---
uid: web-forms/what-is-web-forms
title: Was ist Web Forms | Microsoft-Dokumentation
author: rick-anderson
description: ''
ms.author: riande
ms.date: 02/21/2014
ms.assetid: 5fa1daf9-1161-4cfa-bd4c-658f48b2c229
msc.legacyurl: /web-forms/what-is-web-forms
msc.type: content
ms.openlocfilehash: 19be419c499759713971a6c77674c924867d1bbc
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78516921"
---
# <a name="what-is-web-forms"></a>Was ist Web Forms

ASP.net Web Forms ist Teil des ASP.NET-Webanwendungs-Frameworks und ist in [Visual Studio](https://www.asp.net/downloads)enthalten. Dabei handelt es sich um eines der vier Programmier Modelle, die Sie zum Erstellen von ASP.NET-Webanwendungen verwenden können, die anderen sind ASP.NET MVC-, ASP.net Web Pages-und ASP.net Single-Page-Anwendungen.

Web Forms sind Seiten, die Ihre Benutzer über Ihren Browser anfordern. Diese Seiten können mithilfe einer Kombination aus HTML, Client-Skript, Server Steuerelementen und Servercode geschrieben werden. Wenn Benutzer eine Seite anfordern, werden Sie vom Framework kompiliert und auf dem Server ausgeführt, und dann generiert das Framework das HTML-Markup, das der Browser Rendering ausführen kann. Eine ASP.net Web Forms Seite zeigt dem Benutzerinformationen in einem beliebigen Browser oder Client Gerät an.

Mithilfe von Visual Studio können Sie ASP.net Web Forms erstellen. Mit der integrierten Entwicklungsumgebung (Integrated Development Environment, IDE) von Visual Studio können Sie Server Steuerelemente ziehen und ablegen, um die Web Forms Seite anzulegen. Sie können dann problemlos Eigenschaften, Methoden und Ereignisse für Steuerelemente auf der Seite oder für die Seite selbst festlegen. Diese Eigenschaften, Methoden und Ereignisse werden verwendet, um das Verhalten der Webseite, das Aussehen und Verhalten usw. zu definieren. Zum Schreiben von Servercode, um die Logik für die Seite zu verarbeiten, können Sie eine .NET-Sprache C#wie Visual Basic oder verwenden.

> [!NOTE] 
> 
> Die ASP.net-und Visual Studio-Dokumentation umfasst mehrere Versionen. Themen, in denen Features früherer Versionen hervorgehoben werden, können für Ihre aktuellen Aufgaben und Szenarios mit den neuesten Versionen nützlich sein.

**ASP.net Web Forms:** 

- Basierend auf Microsoft ASP.NET Technologie, bei der Code, der auf dem Server ausgeführt wird, dynamisch eine Webseiten Ausgabe an den Browser oder das Client Gerät generiert.
- Kompatibel mit einem beliebigen Browser oder mobilen Gerät. Eine ASP.NET-Webseite rendert automatisch den richtigen Browser kompatiblen HTML-Code für Funktionen wie Stile, Layout usw.
- Kompatibel mit jeder Sprache, die von der .NET-Common Language Runtime unterstützt wird, z. C#b. Microsoft Visual Basic und Microsoft Visual.
- Basiert auf Microsoft .NET Framework. Dies bietet alle Vorteile des Frameworks, einschließlich einer verwalteten Umgebung, Typsicherheit und Vererbung.
- Flexibel, da Sie Benutzer erstellte Steuerelemente und Steuerelemente von Drittanbietern hinzufügen können.

**ASP.net Web Forms Angebot:** 

- Trennung von HTML und anderem UI-Code von der Anwendungslogik.
- Eine umfangreiche Sammlung von Server Steuerelementen für häufige Aufgaben, einschließlich des Datenzugriffs.
- Leistungsstarke Datenbindung mit hervorragend Tool Unterstützung.
- Unterstützung für Client seitige Skripterstellung, die im Browser ausgeführt wird.
- Unterstützung für eine Vielzahl anderer Funktionen, einschließlich Routing, Sicherheit, Leistung, Internationalisierung, testen, Debuggen, Fehlerbehandlung und Zustands Verwaltung.

## <a name="aspnet-web-forms-helps-you-overcome-challenges"></a>ASP.net Web Forms unterstützt Sie bei der Bewältigung von Problemen

Die Programmierung von Webanwendungen stellt Herausforderungen dar, die in der Regel beim Programmieren herkömmlicher Client basierter Anwendungen nicht auftreten Zu den Herausforderungen gehören:

- **Implementieren einer umfassenden Webbenutzer Oberfläche** : Es kann schwierig und mühsam sein, eine Benutzeroberfläche mit grundlegenden HTML-Funktionen zu entwerfen und zu implementieren. Dies gilt insbesondere dann, wenn die Seite ein komplexes Layout, eine große Menge dynamischer Inhalte und Benutzer interaktive Objekte mit vollem Funktionsumfang aufweist.
- **Trennung von Client und Server** : in einer Webanwendung sind der Client (Browser) und der Server verschiedene Programme, die häufig auf unterschiedlichen Computern ausgeführt werden (und sogar auf unterschiedlichen Betriebssystemen). Folglich werden die beiden Hälften der Anwendung nur sehr wenig Informationen gemeinsam nutzen. Sie können kommunizieren, aber in der Regel nur kleine Blöcke mit einfachen Informationen austauschen.
- Zustands **lose Ausführung** : Wenn ein Webserver eine Anforderung für eine Seite empfängt, sucht er die Seite, verarbeitet sie und sendet Sie an den Browser und verwirft dann alle Seiten Informationen. Wenn der Benutzer die gleiche Seite erneut anfordert, wiederholt der Server die gesamte Sequenz und verarbeitet die Seite neu. Anders ausgedrückt: ein Server hat keinen Arbeitsspeicher von Seiten, die er verarbeitet hat – die Seite ist zustandslos. Wenn also eine Anwendung Informationen zu einer Seite aufbewahren muss, kann die Zustands lose Natur zu einem Problem werden.
- **Unbekannte Client Funktionen** : in vielen Fällen können viele Benutzer mit unterschiedlichen Browsern auf Webanwendungen zugreifen. Browser verfügen über unterschiedliche Funktionen, sodass es schwierig ist, eine Anwendung zu erstellen, die für alle gleichermaßen gut ausgeführt werden kann.
- **Komplikationen beim Datenzugriff** : das Lesen und Schreiben von Daten in eine Datenquelle in herkömmlichen Webanwendungen kann kompliziert und ressourcenintensiv sein.
- **Komplikationen mit Skalierbarkeit** : in vielen Fällen können Webanwendungen, die mit vorhandenen Methoden entworfen wurden, die Skalierbarkeits Ziele aufgrund der fehlenden Kompatibilität zwischen den verschiedenen Komponenten der Anwendung nicht erfüllen. Dies ist häufig ein häufig verwendeter Fehlerpunkt für Anwendungen, die einen hohen Wachstums Aufwand verursachen.

Diese Herausforderungen für Webanwendungen zu erfüllen, kann beträchtliche Zeit und Mühe erfordern. ASP.net Web Forms und das ASP.NET-Framework lösen diese Herausforderungen wie folgt aus:

- **Intuitives, konsistentes Objektmodell** : das ASP.net page Framework stellt ein Objektmodell dar, mit dem Sie Ihre Formulare als Einheit und nicht als separate Client-und Server Teile vorstellen können. In diesem Modell können Sie die Seite auf intuitiver Weise programmieren als in herkömmlichen Webanwendungen, einschließlich der Möglichkeit, Eigenschaften für Seitenelemente festzulegen und auf Ereignisse zu reagieren. Außerdem sind ASP.NET-Server Steuerelemente eine Abstraktion vom physischen Inhalt einer HTML-Seite und von der direkten Interaktion zwischen Browser und Server. Im Allgemeinen können Sie Server Steuerelemente auf die Art und Weise verwenden, wie Sie in einer Client Anwendung mit Steuerelementen arbeiten. Sie müssen sich keine Gedanken darüber machen, wie Sie den HTML-Code erstellen, um die Steuerelemente und deren Inhalte darzustellen und zu verarbeiten
- **Ereignisgesteuerte Programmiermodell** -ASP.net Web Forms das vertraute Modell zum Schreiben von Ereignis Handlern für Ereignisse, die auf dem Client oder auf dem Server auftreten. Das ASP.net page Framework abstrahiert dieses Modell so, dass der zugrunde liegende Mechanismus zum Erfassen eines Ereignisses auf dem Client, zum Übertragen des Ereignisses an den Server und zum Aufrufen der entsprechenden Methode automatisch und unsichtbar ist. Das Ergebnis ist eine klare, leicht geschriebene Code Struktur, die die ereignisgesteuerte Entwicklung unterstützt.
- **Intuitiver Zustands Verwaltung** : das ASP.NET-Seiten Framework verarbeitet automatisch den Zustand Ihrer Seite und der zugehörigen Steuerelemente und bietet explizite Möglichkeiten, den Zustand von anwendungsspezifischen Informationen beizubehalten. Dies wird ohne hohe Nutzung von Server Ressourcen erreicht und kann mit oder ohne Senden von Cookies an den Browser implementiert werden.
- **Browser unabhängige Anwendungen** : das ASP.net page Framework ermöglicht es Ihnen, alle Anwendungslogik auf dem Server zu erstellen, sodass Sie nicht explizit für Unterschiede in Browsern codieren müssen. Allerdings können Sie die Vorteile von browserspezifischen Features nutzen, indem Sie Client seitigen Code schreiben, um eine verbesserte Leistung und ein umfasseneres Client Verhalten bereitzustellen.
- **.NET Framework Common Language Runtime Unterstützung** : das ASP.net page Framework basiert auf dem .NET Framework, sodass das gesamte Framework für jede ASP.NET-Anwendung verfügbar ist. Ihre Anwendungen können in jeder Sprache geschrieben werden, die mit der Laufzeit kompatibel ist. Außerdem wird der Datenzugriff mithilfe der vom .NET Framework bereitgestellten Datenzugriffs Infrastruktur vereinfacht, einschließlich ADO.net.
- **.NET Framework skalierbare Serverleistung** : das ASP.net page Framework ermöglicht es Ihnen, Ihre Webanwendung auf einem Computer mit einem einzelnen Prozessor ordnungsgemäß und ohne komplizierte Änderungen an der Logik der Anwendung zu skalieren.

## <a name="features-of-aspnet-web-forms"></a>Features von ASP.net Web Forms

- **Server Steuerelemente**ASP.NET Webserver-Steuerelemente sind Objekte auf ASP.NET Webseiten, die ausgeführt werden, wenn die Seite angefordert wird, und rendermarkup im Browser. Viele Webserver-Steuerelemente ähneln bekannten HTML-Elementen, z. b. Schaltflächen und Textfeldern. Andere Steuerelemente umfassen komplexes Verhalten, z. b. Kalender Steuerelemente, und Steuerelemente, die Sie verwenden können, um eine Verbindung mit Datenquellen herzustellen und Daten anzuzeigen.
- **Masterseiten**ASP.net mit Masterseiten können Sie ein konsistentes Layout für die Seiten in Ihrer Anwendung erstellen. Eine einzige Masterdseite definiert das Aussehen und das Standardverhalten, das Sie für alle Seiten (oder eine Gruppe von Seiten) in Ihrer Anwendung wünschen. Anschließend können Sie die einzelnen Inhaltsseiten mit dem anzuzeigenden Inhalt erstellen. Wenn Benutzer die Inhaltsseiten anfordern, werden Sie mit der Master Seite zusammengeführt, um eine Ausgabe zu erstellen, die das Layout der Master Seite mit dem Inhalt der Inhaltsseite kombiniert.
- **Arbeiten mit Daten**ASP.net bietet viele Optionen zum Speichern, abrufen und Anzeigen von Daten. In einer ASP.net-Web Forms Anwendung verwenden Sie Daten gebundene Steuerelemente, um die Darstellung oder Eingabe von Daten in Webseiten-Benutzeroberflächen Elementen wie Tabellen und Textfeldern und Dropdown Listen zu automatisieren.
- **Mitgliedschafts**ASP.net Identity speichert die Anmelde Informationen Ihrer Benutzer in einer Datenbank, die von der Anwendung erstellt wurde. Wenn sich Ihre Benutzer anmelden, überprüft die Anwendung ihre Anmelde Informationen, indem Sie die Datenbank liest. Der *Konto* Ordner Ihres Projekts enthält die Dateien, die die verschiedenen Teile der Mitgliedschaft implementieren: registrieren, anmelden, Ändern eines Kennworts und Autorisierungs Zugriff. Darüber hinaus unterstützt ASP.net Web Forms OAuth und OpenID. Diese Authentifizierungs Erweiterungen ermöglichen es Benutzern, sich bei der Website mit vorhandenen Anmelde Informationen anzumelden, wie z. b. Facebook, Twitter, Windows Live und Google. Standardmäßig erstellt die Vorlage eine Mitgliedschafts Datenbank unter Verwendung eines Standarddaten Banknamens in einer Instanz von SQL Server Express localdb, dem Entwicklungsdaten Bank Server, der mit Visual Studio Express 2013 für Web bereit steht.
- **Client Skripts und Client-Frameworks**: Sie können die serverbasierten Features von ASP.net verbessern, indem Sie die Client-Skript-Funktionalität in ASP.net Web Form-Seiten einschließen. Sie können Client Skripts verwenden, um Benutzern eine umfangreichere, reaktionsfähigere Benutzeroberfläche bereitzustellen. Sie können auch Client Skripts verwenden, um asynchrone Aufrufe an den Webserver vorzunehmen, während eine Seite im Browser ausgeführt wird.
- **Routing**-URL-Routing ermöglicht Ihnen das Konfigurieren einer Anwendung für die Annahme von Anforderungs-URLs, die nicht physischen Dateien zugeordnet sind. Eine Anforderungs-URL ist einfach die URL, die ein Benutzer in seinen Browser eingibt, um eine Seite auf Ihrer Website zu finden. Sie verwenden das Routing, um URLs zu definieren, die für Benutzer semantisch aussagekräftig sind und die bei der Suchmaschinenoptimierung (Search Engine Optimization, SEO) helfen können.
- **State Management**-ASP.net Web Forms umfasst mehrere Optionen, mit denen Sie Daten auf Seitenbasis und auf Anwendungs weite Weise beibehalten.
- **Sicherheit**: ein wichtiger Bestandteil der Entwicklung einer sichereren Anwendung ist das Verständnis der Bedrohungen. Microsoft hat eine Möglichkeit zum Kategorisieren von Bedrohungen entwickelt: Spoofing, Manipulation, ablehnen, Offenlegung von Informationen, Denial-of-Service, Erhöhung von Berechtigungen (stride). In ASP.net Web Forms können Sie Erweiterbarkeits Punkte und Konfigurationsoptionen hinzufügen, die es Ihnen ermöglichen, verschiedene Sicherheits Verhaltensweisen in ASP.net Web Forms anzupassen.
- **Leistung**: die Leistung kann ein wichtiger Faktor bei einer erfolgreichen Website oder einem erfolgreichen Projekt sein. ASP.net Web Forms ermöglicht es Ihnen, die Leistung in Bezug auf die Verarbeitung von Seiten-und Server Steuerelementen, Zustands Verwaltung, Datenzugriff, Anwendungskonfiguration und-laden sowie effiziente Codierungsverfahren zu ändern.
- **Internationalisierung**: ASP.net Web Forms ermöglicht Ihnen das Erstellen von Webseiten, die Inhalte und andere Daten basierend auf der bevorzugten Spracheinstellung für den Browser oder basierend auf der expliziten Sprachauswahl des Benutzers abrufen können. Inhalt und andere Daten werden als Ressourcen bezeichnet, und diese Daten können in Ressourcen Dateien oder anderen Quellen gespeichert werden. Auf einer ASP.net-Web Forms Seite konfigurieren Sie Steuerelemente, um Ihre Eigenschaftswerte aus Ressourcen zu erhalten. Zur Laufzeit werden die Ressourcen Ausdrücke durch Ressourcen aus der entsprechenden lokalisierten Ressourcen Datei ersetzt.
- **Debuggen und Fehlerbehandlung**: ASP.NET enthält Funktionen, die Ihnen bei der Diagnose von Problemen helfen, die möglicherweise in Ihrer Web Forms Anwendung auftreten. Debuggen und Fehlerbehandlung werden innerhalb von ASP.net-Web Forms gut unterstützt, sodass Ihre Anwendungen effektiv kompiliert und ausgeführt werden.
- **Bereitstellung und Hosting**: Visual Studio, ASP.net, Azure und IIS stellen Tools bereit, die Ihnen beim Bereitstellen und Hosting Ihrer Web Forms Anwendung helfen.

## <a name="deciding-when-to-create-a-web-forms-application"></a>Entscheiden, wann eine Web Forms Anwendung erstellt werden soll

Sie müssen sorgfältig überlegen, ob Sie eine Webanwendung mit dem ASP.net-Web Forms Modell oder einem anderen Modell (z. b. dem ASP.NET MVC-Framework) implementieren. Das MVC-Framework ersetzt nicht das Web Forms Modell. Sie können beide Frameworks für Webanwendungen verwenden. Bevor Sie sich entscheiden, das Web Forms Modell oder das MVC-Framework für eine bestimmte Website zu verwenden, wiegen Sie die Vorteile der einzelnen Ansätze.

### <a name="advantages-of-a-web-forms-based-web-application"></a>Vorteile einer Web Forms basierten Webanwendung

Das Web Forms basierte Framework bietet die folgenden Vorteile:

- Es unterstützt ein Ereignis Modell, das den Zustand über HTTP beibehält, was die Entwicklung von Webanwendungen für die Entwicklung unterstützt. Die Web Forms basierte Anwendung bietet Dutzende von Ereignissen, die in Hunderten von Server Steuerelementen unterstützt werden.
- Dabei wird ein Seiten Controller Muster verwendet, das einzelnen Seitenfunktionen hinzufügt. Weitere Informationen finden Sie unter [Page Controller](https://go.microsoft.com/fwlink/?LinkId=106359 "Seiten Controller") auf der MSDN-Website.
- Sie verwendet Ansichts Zustand oder serverbasierte Formulare, die die Verwaltung von Zustandsinformationen vereinfachen.
- Dies funktioniert gut für kleine Teams von Webentwicklern und Designern, die die große Anzahl von Komponenten nutzen möchten, die für die schnelle Anwendungsentwicklung zur Verfügung stehen.
- Im Allgemeinen ist es für die Anwendungsentwicklung weniger komplex, da die Komponenten (die **Seiten** Klasse, Steuerelemente usw.) eng integriert sind und normalerweise weniger Code als das MVC-Modell benötigen.

### <a name="advantages-of-an-mvc-based-web-application"></a>Vorteile einer MVC-basierten Webanwendung

Das ASP.NET-MVC-Framework bietet die folgenden Vorteile:

- Es vereinfacht die Verwaltung der Komplexität durch Aufteilen einer Anwendung in das Modell, die Ansicht und den Controller.
- Sie verwendet weder den Ansichts Zustand noch serverbasierte Formulare. Dadurch ist das MVC-Framework ideal für Entwickler, die die volle Kontrolle über das Verhalten einer Anwendung wünschen.
- Dabei wird ein Front-Controller-Muster verwendet, das Webanwendungs Anforderungen über einen einzelnen Controller verarbeitet. Dies ermöglicht Ihnen das Entwerfen einer Anwendung, die eine umfangreiche Routing Infrastruktur unterstützt. Weitere Informationen finden Sie unter [Front Controller](https://go.microsoft.com/fwlink/?LinkId=106357 "Front-Controller") auf der MSDN-Website.
- Sie bietet bessere Unterstützung für die Test gesteuerte Entwicklung (Test-gesteuerte Entwicklung, TDD).
- Dies funktioniert gut für Webanwendungen, die von großen Teams von Entwicklern und Webdesignern unterstützt werden, die ein hohes Maß an Kontrolle über das Anwendungsverhalten benötigen.
