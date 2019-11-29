---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
title: Entwickeln realer Cloud-apps mit Azure | Microsoft-Dokumentation
author: MikeWasson
description: Dieses e-book führt Sie durch einen musterbasierten Ansatz zum Aufbau realer cloudlösungen. Die Muster gelten sowohl für den Entwicklungsprozess als auch für eine...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: accfa16a-ab15-4c26-9ad4-babdc2a77d2e
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
msc.type: authoredcontent
ms.openlocfilehash: 8a4ef3aa37a9296e92fbeb513968e3abeee072d0
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74585522"
---
# <a name="building-real-world-cloud-apps-with-azure"></a>Entwickeln realer Cloud-apps mit Azure

von [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Herunterladen des IT-Projekts](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) oder [herunterladen des E-Books](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Dieses e-book führt Sie durch einen musterbasierten Ansatz zum Aufbau realer cloudlösungen. Die Muster gelten sowohl für den Entwicklungsprozess als auch für Architektur-und Codierungsverfahren.
> 
> Der Inhalt basiert auf einer Präsentation, die von Scott Guthrie entwickelt wurde und die von ihm auf der Norwegian Developers Conference (NDC) im Juni 2013 ([Teil 1](http://vimeo.com/68215538), [Teil 2](http://vimeo.com/68215602)) und bei Microsoft Tech Ed Australia im September 2013 ([Teil 1](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR324), [Teil 2](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR325)) geliefert wurde. [Viele andere](more-patterns-and-guidance.md#acknowledgments) Benutzer haben den Inhalt aktualisiert und erweitert, während er vom Video in das geschriebene Formular übergeht.

## <a name="intended-audience"></a>Beabsichtigte Zielgruppe

Entwickler, die sich mit der Entwicklung für die Cloud auskennen, in Erwägung ziehen, in die Cloud zu wechseln oder mit der cloudentwicklung noch nicht vertraut sind, finden hier einen präzisen Überblick über die wichtigsten Konzepte und Vorgehensweisen, die Sie kennen müssen. Die Konzepte werden mit konkreten Beispielen veranschaulicht, und jedes Kapitel verknüpft sich mit anderen Ressourcen, um ausführlichere Informationen zu erhalten. Die Beispiele und die Links zu zusätzlichen Ressourcen sind für Microsoft-Frameworks und-Dienste, die veranschaulichten Grundsätze gelten jedoch auch für andere Webentwicklungs-Frameworks und cloudumgebungen.

Entwickler, die bereits für die Cloud entwickeln, können hier Ideen finden, die Ihnen helfen, Sie erfolgreicher zu gestalten. Jedes Kapitel in der Reihe kann unabhängig voneinander gelesen werden, sodass Sie die gewünschten Themen auswählen und auswählen können.

Jede Person, die Scott Guthrie *mit Azure* Presentation erstellt hat und weitere Details und aktualisierte Informationen wünscht, finden Sie hier.

<a id="patterns"></a>
## <a name="cloud-development-patterns"></a>Cloudentwicklungmuster

Dieses e-book erläutert 13 Empfohlene Muster für die cloudentwicklung. "Pattern" wird hier im Allgemeinen verwendet, um eine empfohlene Vorgehensweise zu erreichen: wie am besten ist es, Cloud-apps zu entwickeln, zu entwerfen und zu codieren. Dabei handelt es sich um Schlüssel Muster, die Ihnen helfen, "in die boxboxbox" zu kommen, wenn Sie Sie befolgen.

- [Automatisieren Sie alles](automate-everything.md).

    - Verwenden von Skripts zum Maximieren der Effizienz und minimieren von Fehlern in wiederkehrenden Prozessen
    - Demo: Azure-Verwaltungs Skripts.
- [Quell](source-control.md)Code Verwaltung. 

    - Einrichten der Verzweigungs Struktur in der Quell Code Verwaltung, um den devops-Workflow zu vereinfachen.
    - Demo: Fügen Sie Skripts zur Quell Code Verwaltung hinzu.
    - Demo: halten Sie sensible Daten aus der Quell Code Verwaltung heraus.
    - Demo: Verwenden Sie git in Visual Studio.
- [Continuous Integration und Continuous Delivery](continuous-integration-and-continuous-delivery.md). 

    - Automatisieren Sie Build und Bereitstellung mit jedem Eincheck Vorgang für die Quell Code Verwaltung.
- [Bewährte Methoden für die Webentwicklung](web-development-best-practices.md). 

    - Sorgen Sie für Zustands lose webtier
    - Demo: Skalierung und automatische Skalierung in Web-Apps in Azure App Service.
    - Vermeiden Sie den Sitzungszustand.
    - Verwenden Sie ein CDN mit einem Fall Back, wenn das CDN nicht verfügbar ist.
    - Verwenden Sie das asynchrone Programmiermodell.
    - Demo: Async in ASP.NET MVC und Entity Framework.
- [Einmaliges Anmelden](single-sign-on.md). 

    - Einführung in Azure Active Directory.
    - Demo: Erstellen einer ASP.net-APP, die Azure Active Directory verwendet.
- [Optionen für die Datenspeicherung](data-storage-options.md). 

    - Typen von Daten speichern.
    - Wählen Sie den richtigen Datenspeicher aus.
    - Demo: Azure SQL-Datenbank.
- [Strategien für die Daten Partitionierung](data-partitioning-strategies.md). 

    - Partitionieren Sie Daten vertikal, horizontal oder beides, um das Skalieren einer relationalen Datenbank zu vereinfachen.
- [Unstrukturierter BLOB-Speicher](unstructured-blob-storage.md). 

    - Speichern Sie Dateien in der Cloud, indem Sie den BLOB-Dienst verwenden.
    - Demo: Verwenden von BLOB Storage in der Korrektur der IT-app.
- [Entwurf, um Fehler zu überstehen](design-to-survive-failures.md). 

    - Arten von Fehlern.
    - Fehlerbereich.
    - Grundlegendes zu SLAs.
- [Überwachung und Telemetrie](monitoring-and-telemetry.md). 

    - Warum sollten Sie eine telemetrieapp kaufen und eigenen Code schreiben, um Ihre APP zu instrumentieren.
    - Demo: New Relic für Azure
    - Demo: Protokollieren von Code in der Fix-app.
    - Demo: Abhängigkeitsinjektion in der Fix-it-app.
    - Demo: integrierte Protokollierungs Unterstützung in Azure.
- [Behandlung vorübergehender Fehler](transient-fault-handling.md). 

    - Verwenden Sie intelligente Wiederholungs-/Backoff-Logik, um die Auswirkungen vorübergehender Fehler zu mindern.
    - Demo: Wiederholung/Backoff in Entity Framework 6.
- [Verteiltes Zwischenspeichern](distributed-caching.md). 

    - Verbessern Sie die Skalierbarkeit, und reduzieren Sie die Kosten für die Datenbanktransaktion durch die
- [Warteschlangen zentriertes Arbeitsmuster](queue-centric-work-pattern.md). 

    - Aktivieren der Hochverfügbarkeit und verbessern der Skalierbarkeit durch lose Kopplung von Web-und workerebenen.
    - Demo: Azure Storage-Warteschlangen in der Korrektur der IT-app.
- [Weitere Cloud-App-Muster und Anleitungen](more-patterns-and-guidance.md).
- [Anhang: Fix It-Beispielanwendung](the-fix-it-sample-application.md)

    - Bekannte Probleme
    - Bewährte Methoden
    - Herunterladen, erstellen, ausführen und bereitstellen.

Diese Muster gelten für alle cloudumgebungen, aber wir veranschaulichen Sie anhand von Beispielen, die auf Microsoft-Technologien und-Diensten basieren, wie z. b. Visual Studio, Team Foundation Service, ASP.net und Azure.

In diesem weiteren Verlauf dieses Kapitels werden die Fehlerbehebung für die IT-Beispielanwendung und die Web-Apps in Azure App Service Cloud-Umgebung vorgestellt, in der die Korrektur der IT-App

<a id="fixit"></a>
## <a name="the-fix-it-sample-application"></a>Die Anwendung zum Beheben der IT-Beispiele

Die meisten Screenshots und Codebeispiele in diesem e-book basieren auf der Lösung, die von [Scott Guthrie](https://weblogs.asp.net/scottgu/) ursprünglich entwickelt wurde, um empfohlene Entwicklungsmuster und-Methoden für Cloud-apps zu veranschaulichen.

![Reparieren der Startseite der IT-App](introduction/_static/image1.png)

Die Beispiel-APP ist ein einfaches Arbeits Element-Ticketsystem. Wenn Sie etwas Festes benötigen, erstellen Sie ein Ticket und weisen es einer Person zu. andere Benutzer können sich anmelden und die zugewiesenen Tickets anzeigen und Tickets als abgeschlossen markieren, wenn die Arbeit erledigt ist.

Dabei handelt es sich um ein standardmäßiges Visual Studio-Webprojekt. Sie basiert auf ASP.NET MVC und verwendet eine SQL Server Datenbank. Sie kann lokal in IIS Express ausgeführt und auf einer Azure-Website bereitgestellt werden, die in der Cloud ausgeführt werden kann. Sie können sich mithilfe der Formular Authentifizierung und einer lokalen Datenbank oder mithilfe eines sozialen Anbieters wie Google anmelden. (Später wird auch erläutert, wie Sie sich mit einem Active Directory Organisations Konto anmelden.)

![Anmeldeseite](introduction/_static/image2.png)

Sobald Sie angemeldet sind, können Sie ein Ticket erstellen, einem anderen Benutzer zuweisen und ein Bild davon hochladen, was korrigiert werden soll.

![Erstellen einer Korrektur](introduction/_static/image3.png)

![Korrektur der erstellten IT-Aufgabe](introduction/_static/image4.png)

Sie können den Fortschritt der von Ihnen erstellten Arbeitsaufgaben nachverfolgen, die Ihnen zugewiesenen Tickets anzeigen, ticketdetails anzeigen und Elemente als abgeschlossen markieren.

Dies ist eine sehr einfache APP aus einer Funktions Perspektive, aber Sie werden erfahren, wie Sie Sie erstellen können, damit Sie für Millionen von Benutzern skaliert werden kann, und Sie sind anfällig für Daten Bankausfälle und Verbindungs Beendigungen. Außerdem erfahren Sie, wie Sie einen automatisierten und agilen Entwicklungs Workflow erstellen, der Ihnen ermöglicht, einfach zu beginnen und die APP besser und besser zu gestalten, indem Sie den Entwicklungszyklen effizient und schnell iterieren.

<a id="waws"></a>
## <a name="web-apps-in-azure-app-service"></a>Web-Apps in Azure App Service

Die Cloud-Umgebung, die für die Korrektur der IT-Anwendung verwendet wird, ist ein Dienst von Azure, den wir als Websites bezeichnen. Dieser Dienst ist eine Methode, mit der Sie Ihre eigene Web-App in Azure hosten können, ohne VMS erstellen und aktualisieren, installieren und Konfigurieren von IIS usw. Wir hosten Ihre Website auf unseren VMS und stellen automatisch Sicherung und Wiederherstellung sowie andere Dienste für Sie bereit. Der Websites-Dienst funktioniert mit ASP.net, Node. js, PHP und python. Sie ermöglicht eine sehr schnelle Bereitstellung mithilfe von Visual Studio, Web deploy, FTP, git oder TFS. Es dauert in der Regel nur wenige Sekunden zwischen dem Start einer Bereitstellung und dem Zeitpunkt, zu dem das Update über das Internet verfügbar ist. Der Einstieg ist kostenlos, und Sie können den Datenverkehr zentral hochskalieren.

Im Hintergrund bieten Web-Apps in Azure App Service viele Architekturkomponenten und Features, die Sie selbst erstellen müssten, wenn Sie eine Website mit IIS auf Ihren eigenen virtuellen Computern hosten möchten. Bei einer Komponente handelt es sich um einen Bereitstellungs Endpunkt, der automatisch IIS konfiguriert und die Anwendung auf so vielen virtuellen Computern installiert, auf denen Sie Ihre Website ausführen möchten.

![Bereitstellungs Dienst](introduction/_static/image5.png)

Wenn ein Benutzer auf die Website trifft, werden die IIS-VMS nicht direkt auf die IIS-VMS angewendet, sondern durch den [Anwendungs Anforderungs Routing (arr)](https://www.iis.net/downloads/microsoft/application-request-routing) Load Balancer. Sie können diese mit ihren eigenen Servern verwenden, aber der Vorteil liegt darin, dass Sie automatisch für Sie eingerichtet werden. Sie verwenden eine intelligente heuristische, bei der Faktoren wie Sitzungs Affinität, Warteschlangen Tiefe in IIS und CPU-Auslastung auf den einzelnen Computern berücksichtigt werden, um den Datenverkehr an die VMs weiterzuleiten, die die Website hosten.

![ARR-Lastenausgleich](introduction/_static/image6.png)

Wenn ein Computer ausfällt, ruft Azure ihn automatisch aus der Rotation ab, startet eine neue VM-Instanz und leitet den Datenverkehr an die neue Instanz weiter, ohne dass die Zeit für Ihre Anwendung nicht ausfällt.

![Automatische Wiederherstellung nach Computerfehler](introduction/_static/image7.png)

All dies erfolgt automatisch. Sie müssen lediglich eine Website erstellen und Ihre Anwendung mithilfe von Windows PowerShell, Visual Studio oder dem Azure-Verwaltungs Portal bereitstellen.

Ein schnelles und einfaches Tutorial, das zeigt, wie Sie eine Webanwendung in Visual Studio erstellen und auf einer Azure-Website bereitstellen, finden Sie unter Erste Schritte [mit Azure und ASP.net](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).

<a id="summary"></a>
## <a name="summary"></a>Summary

Diese Einführung bietet eine Liste der Themen, die im Buch behandelt werden, Screenshots der Beispielanwendung und eine kurze Übersicht über die Web-Apps in Azure App Service Cloud-Umgebung. Einer der großen Vorteile bei der Entwicklung von apps in und für die Cloud besteht darin, dass sich wiederholende Entwicklungsaufgaben, z. b. das Erstellen einer Testumgebung und die Bereitstellung Ihres Codes, leicht automatisieren können. Dies ist der Gegenstand des [nächsten Kapitels](automate-everything.md).

## <a name="resources"></a>Ressourcen

Weitere Informationen zu den in diesem Kapitel behandelten Themen finden Sie in den folgenden Ressourcen.

Dokumentation:

- [Web-Apps in Azure App Service](https://azure.microsoft.com/services/app-service/web/). Portal Seite für die Azure-Dokumentation zu Web-Apps.
- [Web-Apps, Cloud Services und VMS: Wann sollte verwendet werden?](https://azure.microsoft.com/documentation/articles/choose-web-site-cloud-service-vm/) Waws, wie in diesem Kapitel gezeigt, ist nur eine von drei Möglichkeiten, wie Sie Web-Apps in Azure ausführen können. In diesem Artikel werden die Unterschiede zwischen den drei Methoden erläutert, und es wird erläutert, wie Sie entscheiden, welche Option für Ihr Szenario geeignet ist. Wie Websites ist Cloud Services ein Feature von Azure. VMS sind ein IaaS-Feature. Eine Erläuterung von Pas im Vergleich zu IaaS finden Sie im Kapitel zu [Daten Optionen](data-storage-options.md#paasiaas) .

Videos:

- [Scott Guthrie beginnt mit Schritt 0: Was ist die Azure-Cloud OS?](https://azure.microsoft.com/documentation/videos/what-is-the-cloud-os-scottgu/)
- [Website Architektur: Stefan Schackow](https://azure.microsoft.com/documentation/videos/why-azure-web-sites-plus-architecture/).
- [Azure Websites Internals mit NIR mashkowski](https://channel9.msdn.com/Shows/Web+Camps+TV/Windows-Azure-Web-Sites-Internals-with-Nir-Mashkowski).

> [!div class="step-by-step"]
> [Nächste](automate-everything.md)
