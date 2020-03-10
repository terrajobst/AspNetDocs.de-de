---
uid: mvc/overview/older-versions-1/overview/asp-net-mvc-overview
title: Übersicht über ASP.NET MVC | Microsoft-Dokumentation
author: microsoft
description: Erfahren Sie mehr über die Unterschiede zwischen ASP.NET MVC-Anwendungen und ASP.net-Web Forms Anwendungen. Erfahren Sie, wie Sie entscheiden, wann eine ASP.NET MVC-Anwendung erstellt werden soll.
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 2dcb44a4-5cbf-4d62-b363-718104082d86
msc.legacyurl: /mvc/overview/older-versions-1/overview/asp-net-mvc-overview
msc.type: authoredcontent
ms.openlocfilehash: 73965c71f37de13e3813df089a253fde528ea7ee
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78435489"
---
# <a name="aspnet-mvc-overview"></a>Übersicht über ASP.NET MVC

von [Microsoft](https://github.com/microsoft)

> Erfahren Sie mehr über die Unterschiede zwischen ASP.NET MVC-Anwendungen und ASP.net-Web Forms Anwendungen. Erfahren Sie, wie Sie entscheiden, wann eine ASP.NET MVC-Anwendung erstellt werden soll.

Das Architekturmuster Model-View-Controller (MVC) trennt eine Anwendung in drei Hauptkomponenten: das Modell, die Ansicht und den Controller. Das ASP.NET-MVC-Framework bietet eine Alternative zum ASP.net-Web Forms Muster zum Erstellen von MVC-basierten Webanwendungen. Das ASP.NET-MVC-Framework ist ein schlankes, hochgradig testbares Präsentations Framework, das (wie bei Web Forms basierten Anwendungen) in vorhandene ASP.NET-Features wie Masterseiten und Mitgliedschafts basierte Authentifizierung integriert ist. Das MVC-Framework ist im Namespace " **System. Web. MVC** " definiert und ist ein grundlegender, unterstützter Teil des **System. Web** -Namespace.   
  
MVC ist ein Standard Entwurfsmuster, mit dem viele Entwickler vertraut sind. Einige Arten von Webanwendungen profitieren vom MVC-Framework. Andere verwenden weiterhin das herkömmliche ASP.NET-Anwendungs Muster, das auf Web Forms und Postbacks basiert. Andere Arten von Webanwendungen werden die beiden Ansätze kombinieren. keines dieser Ansätze schließt die andere aus.   
  
Das MVC-Framework umfasst die folgenden Komponenten:

[![Aufrufen einer Controller Aktion, die einen Parameterwert erwartet](asp-net-mvc-overview/_static/image1.jpg)](asp-net-mvc-overview/_static/image1.png)

**Abbildung 01**: Aufrufen einer Controller Aktion, die einen Parameterwert erwartet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](asp-net-mvc-overview/_static/image2.png))

- **Modelle**. Modell Objekte sind die Teile der Anwendung, die die Logik für die Daten Domäne der Anwendung implementieren. Häufig werden Modell Objekte in einer Datenbank abgerufen und gespeichert. Ein Product-Objekt kann z. b. Informationen aus einer Datenbank abrufen, darauf arbeiten und anschließend aktualisierte Informationen zurück in eine Products-Tabelle in SQL Server schreiben.

In kleinen Anwendungen ist das Modell oftmals eine konzeptionelle Trennung anstelle eines physischen Modells. Wenn die Anwendung z. b. nur ein DataSet liest und an die Ansicht sendet, verfügt die Anwendung nicht über eine physische Modell Ebene und zugehörige Klassen. In diesem Fall übernimmt das DataSet die Rolle eines Modell Objekts.

- **Sichten**. Ansichten sind die Komponenten, die die Benutzeroberfläche der Anwendung anzeigen. In der Regel wird diese Benutzeroberfläche aus den Modelldaten erstellt. Ein Beispiel wäre eine Bearbeitungs Ansicht einer Produkttabelle, in der Textfelder, Dropdown Listen und Kontrollkästchen auf Grundlage des aktuellen Zustands eines Products-Objekts angezeigt werden.

- **Controller**. Controller sind die Komponenten, die die Benutzerinteraktion verarbeiten, mit dem Modell arbeiten und letztendlich eine Ansicht zum Rendering auswählen, die die Benutzeroberfläche anzeigt. In einer MVC-Anwendung zeigt die Ansicht nur Informationen an. Benutzereingaben und -interaktionen werden vom Controller verarbeitet und beantwortet. Der Controller verarbeitet z. b. Abfrage Zeichenfolgen-Werte und übergibt diese Werte an das Modell, das wiederum die Datenbank mithilfe der-Werte abfragt.

Das MVC-Muster hilft Ihnen beim Erstellen von Anwendungen, die die verschiedenen Aspekte der Anwendung (Eingabe Logik, Geschäftslogik und UI-Logik) trennen, während gleichzeitig eine lose Kopplung zwischen diesen Elementen ermöglicht wird. Das Muster gibt an, wo sich jede Art von Logik in der Anwendung befinden sollte. Die Benutzeroberflächenlogik ist Teil der Ansicht (view). Die Eingabelogik gehört zum Controller. Die Geschäftslogik gehört zum Modell. Diese Trennung hilft Ihnen bei der Verwaltung der Komplexität, wenn Sie eine Anwendung erstellen, da Sie sich auf einen Aspekt der Implementierung gleichzeitig konzentrieren kann. Beispielsweise können Sie sich auf die Ansicht konzentrieren, ohne abhängig von der Geschäftslogik.   
  
Zusätzlich zur Verwaltung der Komplexität erleichtert das MVC-Muster das Testen von Anwendungen als das Testen einer Web Forms basierten ASP.NET-Webanwendung. Beispielsweise wird in einer Web Forms basierten ASP.NET-Webanwendung eine einzelne Klasse verwendet, um die Ausgabe anzuzeigen und auf Benutzereingaben zu reagieren. Das Schreiben automatisierter Tests für Web Forms-basierte ASP.NET-Anwendungen kann komplex sein, da Sie zum Testen einer einzelnen Seite die Seiten Klasse, alle untergeordneten Steuerelemente und zusätzliche abhängige Klassen in der Anwendung instanziieren müssen. Da so viele Klassen instanziiert werden, um die Seite auszuführen, kann es schwierig sein, Tests zu schreiben, die sich ausschließlich auf einzelne Teile der Anwendung konzentrieren. Tests für Web Forms basierte ASP.NET-Anwendungen können daher schwieriger zu implementieren sein als Tests in einer MVC-Anwendung. Außerdem ist für Tests in einer Web Forms basierten ASP.NET-Anwendung ein Webserver erforderlich. Das MVC-Framework entkoppelt die Komponenten und nutzt Schnittstellen stark. Dadurch können einzelne Komponenten isoliert vom Rest des Frameworks getestet werden.   
  
Die lose Kopplung zwischen den drei Hauptkomponenten einer MVC-Anwendung fördert auch die parallele Entwicklung. Beispielsweise kann ein Entwickler an der Ansicht arbeiten, ein zweiter Entwickler kann an der Controller Logik arbeiten, und ein Dritter Entwickler kann sich auf die Geschäftslogik im Modell konzentrieren.

## <a name="deciding-when-to-create-an-mvc-application"></a>Entscheiden, wann eine MVC-Anwendung erstellt werden soll

Sie müssen sorgfältig überlegen, ob eine Webanwendung mit dem ASP.NET MVC-Framework oder mit dem ASP.net-Web Forms Modell implementiert werden soll. Das MVC-Framework ersetzt nicht das Web Forms Modell. Sie können beide Frameworks für Webanwendungen verwenden. (Wenn Sie über Web Forms basierte Anwendungen verfügen, funktionieren diese weiterhin genauso wie immer.)   
  
Bevor Sie sich entscheiden, das MVC-Framework oder das Web Forms Modell für eine bestimmte Website zu verwenden, wiegen Sie die Vorteile der einzelnen Ansätze.

### <a name="advantages-of-an-mvc-based-web-application"></a>Vorteile einer MVC-basierten Webanwendung

Das ASP.NET-MVC-Framework bietet die folgenden Vorteile:

- Es vereinfacht die Verwaltung der Komplexität durch Aufteilen einer Anwendung in das Modell, die Ansicht und den Controller.
- Sie verwendet weder den Ansichts Zustand noch serverbasierte Formulare. Dadurch ist das MVC-Framework ideal für Entwickler, die die volle Kontrolle über das Verhalten einer Anwendung wünschen.
- Dabei wird ein Front-Controller-Muster verwendet, das Webanwendungs Anforderungen über einen einzelnen Controller verarbeitet. Dies ermöglicht Ihnen das Entwerfen einer Anwendung, die eine umfangreiche Routing Infrastruktur unterstützt. Weitere Informationen finden Sie unter [Front Controller](https://go.microsoft.com/fwlink/?LinkId=106357 "Front-Controller") auf der MSDN-Website.
- Sie bietet bessere Unterstützung für die Test gesteuerte Entwicklung (Test-gesteuerte Entwicklung, TDD).
- Dies funktioniert gut für Webanwendungen, die von großen Teams von Entwicklern und Webdesignern unterstützt werden, die ein hohes Maß an Kontrolle über das Anwendungsverhalten benötigen.

### <a name="advantages-of-a-web-forms-based-web-application"></a>Vorteile einer Web Forms basierten Webanwendung

Das Web Forms basierte Framework bietet die folgenden Vorteile:

- Es unterstützt ein Ereignis Modell, das den Zustand über HTTP beibehält, was die Entwicklung von Webanwendungen für die Entwicklung unterstützt. Die Web Forms basierte Anwendung bietet Dutzende von Ereignissen, die in Hunderten von Server Steuerelementen unterstützt werden.
- Dabei wird ein Seiten Controller Muster verwendet, das einzelnen Seitenfunktionen hinzufügt. Weitere Informationen finden Sie unter [Page Controller](https://go.microsoft.com/fwlink/?LinkId=106359 "Seiten Controller") auf der MSDN-Website.
- Sie verwendet Ansichts Zustand oder serverbasierte Formulare, die die Verwaltung von Zustandsinformationen vereinfachen.
- Dies funktioniert gut für kleine Teams von Webentwicklern und Designern, die die große Anzahl von Komponenten nutzen möchten, die für die schnelle Anwendungsentwicklung zur Verfügung stehen.
- Im Allgemeinen ist es für die Anwendungsentwicklung weniger komplex, da die Komponenten (die **Seiten** Klasse, Steuerelemente usw.) eng integriert sind und normalerweise weniger Code als das MVC-Modell benötigen.

## <a name="features-of-the-aspnet-mvc-framework"></a>Features des ASP.NET-MVC-Frameworks

Das ASP.NET-MVC-Framework bietet die folgenden Features:

- Die Trennung von Anwendungsaufgaben (Eingabe Logik, Geschäftslogik und Benutzeroberflächen Logik), Testability und Test gesteuerte Entwicklung (Test-gesteuerte Entwicklung, TDD) standardmäßig. Alle kernverträge im MVC-Framework sind Schnittstellen basiert und können mithilfe von Pseudo Objekten getestet werden, bei denen es sich um simulierte Objekte handelt, die das Verhalten tatsächlicher Objekte in der Anwendung imitieren. Sie können die Anwendung Komponententests ausführen, ohne die Controller in einem ASP.NET-Prozess ausführen zu müssen, wodurch Komponententests schnell und flexibel werden. Sie können jedes Komponenten Test Framework verwenden, das mit dem .NET Framework kompatibel ist.
- Ein erweiterbares und austauschbares Framework. Die Komponenten des ASP.NET-MVC-Frameworks sind so konzipiert, dass Sie problemlos ersetzt oder angepasst werden können. Sie können Ihre eigene Ansichts-Engine, URL-Routing Richtlinie, aktionsmethodenparameterserialisierung und andere Komponenten einbinden. Das ASP.NET-MVC-Framework unterstützt auch die Verwendung von "Abhängigkeitsinjektion (di)" und "Inversion of Control" (IOC)-Container Modellen. DI ermöglicht das Einfügen von Objekten in eine Klasse, anstatt sich auf die Klasse zu verlassen, um das Objekt selbst zu erstellen. IOC gibt an, dass die ersten Objekte das zweite Objekt aus einer externen Quelle, z. b. einer Konfigurationsdatei, erhalten, wenn ein Objekt ein anderes Objekt erfordert. Dies erleichtert das Testen.
- Eine leistungsstarke URL-Zuordnungskomponente, mit der Sie Anwendungen erstellen können, die über verständliche und durchsuchbare URLs verfügen. URLs müssen keine Dateinamen Erweiterungen enthalten und sind für die Unterstützung von URL-Benennungs Mustern konzipiert, die sich gut für die Suchmaschinenoptimierung (Search Engine Optimization, SEO) und die Representational State Transfer (Rest)-Adressierung eignen.
- Unterstützung für das Verwenden des Markups in vorhandenen ASP.net page (ASPX-Dateien), Benutzer Steuerelement (ASCX-Dateien) und Masterseiten (Master Dateien) Markup Dateien als Ansichts Vorlagen. Sie können vorhandene ASP.NET-Funktionen mit dem ASP.NET-MVC-Framework verwenden, z. b. für die Verwendung von nativen Masterseiten, Inline Ausdrücke (&lt;% =%&gt;), deklarative Server Steuerelemente, Vorlagen, Datenbindung, Lokalisierung usw.
- Unterstützung vorhandener ASP.NET-Funktionen. Mit ASP.NET MVC können Sie Features wie Formular Authentifizierung und Windows-Authentifizierung, URL-Autorisierung, Mitgliedschaft und Rollen, Ausgabe-und Daten Caching, Sitzungs-und Profil Zustands Verwaltung, Integritäts Überwachung, das Konfigurationssystem und den Anbieter verwenden. Architektur.
