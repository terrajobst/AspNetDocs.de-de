---
uid: overview
title: ASP.net Übersicht | Microsoft-Dokumentation
author: rick-anderson
description: Einführung in ASP.net, ein kostenloses Framework zum Erstellen von Websites, Webanwendungen und Web-APIs.
ms.assetid: 3a309468-f1ca-4e51-b9c3-536af79d7a8b
ms.author: riande
ms.date: 08/10/2019
msc.legacyurl: ''
msc.type: content
ms.openlocfilehash: aa4f627bca99f0a7ffbbb53ea45ebdcf0850fd89
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78431877"
---
# <a name="aspnet-overview"></a>Übersicht über ASP.NET

ASP.net ist ein kostenloses Webframework zum entwickeln hervorragend Websites und Webanwendungen mithilfe von HTML, CSS und JavaScript. Sie können auch Web-APIs erstellen und Echtzeittechnologien wie websockets verwenden.

[ASP.net Core](https://docs.microsoft.com/aspnet/core/) ist eine Alternative zu ASP.net.  Weitere Informationen finden [Sie in der Anleitung zur Auswahl zwischen ASP.net und ASP.net Core](https://docs.microsoft.com/aspnet/core/choose-aspnet-framework).

## <a name="get-started"></a>Erste Schritte

Installieren Sie [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2019) Community Edition, eine kostenlose IDE für ASP.net unter Windows.

## <a name="websites-and-web-applications"></a>Websites und Webanwendungen

 ASP.net bietet drei Frameworks zum Erstellen von Webanwendungen: Web Forms, ASP.NET MVC und ASP.net Web Pages. Alle drei Frameworks sind stabil und ausgereift, und Sie können mit jedem von Ihnen hervorragend Webanwendungen erstellen. Unabhängig davon, welches Framework Sie auswählen, erhalten Sie alle Vorteile und Features von ASP.net überall.

Jedes Framework hat einen anderen Entwicklungsstil als Ziel. Die Auswahl hängt von einer Kombination der Programmier Ressourcen (Wissensquelle, Fähigkeiten und Entwicklungserfahrung), der Art der Anwendung, die Sie erstellen, und dem Entwicklungsansatz ab, mit dem Sie vertraut sind.

Im folgenden finden Sie eine Übersicht über die einzelnen Frameworks und einige Ideen zur Auswahl zwischen Ihnen. Wenn Sie eine Video Einführung bevorzugen, finden Sie weitere Informationen unter [Erstellen von Websites mit ASP.net](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/Making-Websites-with-ASPNET) und [Was sind Webtools?](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/what-is-web-tools)

|   | Wenn Sie in | Entwicklungsstil | Ihrem |
|-----------|----------------------|-----------------------------------------------------|----------------|
| Web Forms | Win Forms, WPF, .net | Schnelle Entwicklung mithilfe einer umfangreichen Bibliothek von Steuerelementen, die HTML-Markup Kapseln | Mid-Level, Advanced Rad |
| MVC       | Ruby on Rails, .net  | Vollständige Kontrolle über HTML-Markup, Code und Markup getrennt und leicht zu schreibende Tests. Die beste Wahl für mobile Anwendungen und Single-Page-Anwendungen (Spa). | Mittelwert, erweitert |
| Webseiten  | Klassisches ASP, PHP     | HTML-Markup und Code in derselben Datei | Neu, Mittelwert |

### <a name="web-forms"></a>Web Forms

Mit ASP.net Web Forms können Sie dynamische Websites erstellen, indem Sie ein bekanntes, ereignisgesteuerte Drag & Drop-Modell verwenden. Mit einer Entwurfs Oberfläche und Hunderten von Steuerelementen und Komponenten können Sie schnell ausgereifte, leistungsfähige, UI-gesteuerte Websites mit Datenzugriff erstellen.

[Weitere Informationen zu Web Forms](web-forms/index.md)

### <a name="mvc"></a>MVC

ASP.NET MVC bietet Ihnen eine leistungsstarke, auf Mustern basierende Methode zum Erstellen dynamischer Websites, die eine saubere Trennung von Anliegen ermöglicht und Ihnen die vollständige Kontrolle über Markup für eine angenehme, Agile Entwicklung bietet. ASP.NET MVC umfasst viele Features, die eine schnelle, TDD-freundliche Entwicklung zum Erstellen ausgereifter Anwendungen ermöglichen, die die neuesten Webstandards verwenden.

[Weitere Informationen zu MVC](mvc/index.md)

### <a name="aspnet-web-pages"></a>ASP.NET Web Pages

ASP.net Web Pages und die Razor-Syntax bieten eine schnelle, geeignete und einfachere Möglichkeit, Servercode mit HTML zu kombinieren, um dynamische Webinhalte zu erstellen. Stellen Sie eine Verbindung mit Datenbanken her, fügen Sie Videos hinzu, verknüpfen Sie Sie mit sozialen Netzwerken, und fügen Sie viele weitere Funktionen hinzu, mit denen Sie schöne Websites erstellen können, die den neuesten Webstandards

[Weitere Informationen zu Webseiten](web-pages/index.md)

### <a name="notes-about-web-forms-mvc-and-web-pages"></a>Hinweise zu Web Forms, MVC und Webseiten

Alle drei ASP.NET-Frameworks basieren auf den .NET Framework-und Freigabe Kernfunktionen von .net und von ASP.net. Beispielsweise bieten alle drei Frameworks ein Anmelde Sicherheitsmodell, das auf der Mitgliedschaft basiert, und alle drei verfügen über die gleichen Funktionen für die Verwaltung von Anforderungen, die Handhabung von Sitzungen usw., die Bestandteil der ASP.net-Kernfunktionen sind.

Außerdem sind die drei Frameworks nicht ganz unabhängig, und die Auswahl eines Frameworks schließt die Verwendung einer anderen nicht aus. Da die Frameworks in derselben Webanwendung nebeneinander vorhanden sein können, ist es nicht ungewöhnlich, einzelne Komponenten von Anwendungen zu sehen, die mit verschiedenen Frameworks geschrieben wurden. Beispielsweise können Kundenteile einer APP in MVC entwickelt werden, um das Markup zu optimieren, während der Datenzugriff und administrative Teile in Web Forms entwickelt werden, um Daten Steuerelemente und einfachen Datenzugriff zu nutzen.

## <a name="web-apis"></a>Web-APIs

ASP.net-Web-API ist ein Framework, das das Erstellen von http-Diensten erleichtert, die eine breite Palette von Clients erreichen, einschließlich Browsern und mobilen Geräten. Die ASP.NET-Web-API ist eine ideale Plattform zum Erstellen von RESTful-Anwendungen in .NET Framework.

[Weitere Informationen zur Web-API](web-api/index.md)

<!-- Put first under Web API TOC:  Watch video (9 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/services-and-aspnet -->

## <a name="real-time-technologies"></a>Echtzeittechnologien

ASP.net signalr ist eine neue Bibliothek für ASP.NET-Entwickler, die die Entwicklung von Echt Zeit Webrollen vereinfachen. Signalr ermöglicht die bidirektionale Kommunikation zwischen Server und Client. -Server können Inhalte sofort per Push an verbundene Clients Übertragung, sobald Sie verfügbar sind. Signalr unterstützt websockets und greift auf andere kompatible Techniken für ältere Browser zurück. Signalr umfasst APIs für die Verbindungs Verwaltung (beispielsweise Verbindungs-und Trennungs Ereignisse), das Gruppieren von Verbindungen und die Autorisierung.

[Weitere Informationen zu signalr](signalr/index.md)

<!-- Put first under SignalR TOC:  Watch video (6 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/signalr-and-the-real-time-web -->

## <a name="mobile-apps-and-sites"></a>Mobile Apps und Websites

ASP.net kann Native Mobile Apps mit einem Web-API-Back-End und mobilen Websites mit reaktionsfähigen Design-Frameworks wie Twitter-Bootstrap unterstützen. Wenn Sie eine systemeigene Mobile App erstellen, ist es einfach, eine JSON-basierte Web-API zu erstellen, um den Datenzugriff, die Authentifizierung und Pushbenachrichtigungen für Ihre APP zu verarbeiten. Wenn Sie eine reaktionsfähige Mobile Site entwickeln, können Sie jedes beliebige CSS-Framework oder Open Grid-System verwenden, oder Sie können ein leistungsfähiges mobiles System wie jQuery Mobile oder Sencha und großartige Mobile Anwendungen mit PhoneGap auswählen.

[Weitere Informationen zur Entwicklung von Mobile App und Websites](mobile/overview.md)

<!-- Put first under mobile TOC:  Watch video (11 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/aspnet-and-mobile -->

## <a name="single-page-applications"></a>Single-Page-Webanwendungen

ASP.net Single Page Application (Spa) unterstützt Sie beim Erstellen von Anwendungen, die bedeutende Client seitige Interaktionen mit HTML 5, CSS 3 und JavaScript einschließen. Visual Studio enthält eine Vorlage zum Erstellen von Single-Page-Anwendungen mithilfe von Knockout. js und ASP.net-Web-API. Neben der integrierten Spa-Vorlage stehen auch von der Community erstellte Spa-Vorlagen zum Download zur Verfügung.

[Weitere Informationen zur Entwicklung von einseitigen apps](single-page-application/index.md)

## <a name="webhooks"></a>WebHooks

Webhooks ist ein einfaches http-Muster, das ein einfaches Pub/Sub-Modell zum Verbinden von Web-APIs und Saas-Diensten bereitstellt. Wenn ein Ereignis in einem Dienst auftritt, wird eine Benachrichtigung in Form einer HTTP POST-Anforderung an registrierte Abonnenten gesendet. Die Post-Anforderung enthält Informationen zum Ereignis, das es dem Empfänger ermöglicht, entsprechend zu agieren.

Webhooks werden durch eine große Anzahl von Diensten verfügbar gemacht, darunter Dropbox, GitHub, Instagram, MailChimp, PayPal, Slack, trello und vieles mehr. Ein webhook kann z. b. angeben, dass eine Datei in Dropbox geändert wurde oder für eine Codeänderung in GitHub ein Commit ausgeführt wurde oder eine Zahlung in PayPal initiiert wurde oder eine Karte in trello erstellt wurde.

[Weitere Informationen zu webhooks](webhooks/index.md)

<!--
Create Deployment TOC based on https://www.asp.net/aspnet/overview/deployment
Copy deployment content map to MVC, WebForms, Web Pages, Web API sections.
Copy Web Deployment in Enterprise from WebForms to MVC
Move under ASP.NET Best practices
    What not to do in ASP.NET, and what to do instead https://review.docs.microsoft.cus/aspnet/aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
    Async and await https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/async-and-await
    Building Real World Cloud Apps with Azure https://review.docs.microsoft.com/aspnet/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
    Hands on Lab: Maintainable Azure Websites: Managing Change and Scale https://review.docs.microsoft.com/aspnet/aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale

-->
