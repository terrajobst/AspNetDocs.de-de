---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/asp-net-hosting-options-vb
title: ASP.net-Hostingoptionen (VB) | Microsoft-Dokumentation
author: rick-anderson
description: ASP.NET Webanwendungen werden in der Regel in einer lokalen Entwicklungsumgebung entworfen, erstellt und getestet und müssen in einer Produktionsumgebung bereitgestellt werden...
ms.author: riande
ms.date: 04/01/2009
ms.assetid: 492f5ae2-bad7-4107-89a9-f04a9525dee7
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/asp-net-hosting-options-vb
msc.type: authoredcontent
ms.openlocfilehash: 4293114002aee15ddace1a3f19c240f35e6065f5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78520791"
---
# <a name="aspnet-hosting-options-vb"></a>Optionen zum Hosten von ASP.NET (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[PDF herunterladen](https://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial01_Basics_vb.pdf)

> ASP.NET Webanwendungen werden in der Regel in einer lokalen Entwicklungsumgebung entworfen, erstellt und getestet und müssen in einer Produktionsumgebung bereitgestellt werden, sobald Sie für die Veröffentlichung bereit ist. Dieses Tutorial bietet eine allgemeine Übersicht über den Bereitstellungs Prozess und dient als Einführung in diese tutorialreihe.

## <a name="introduction"></a>Einführung

Webanwendungen werden in der Regel in einer Entwicklungsumgebung entworfen, erstellt und getestet, die nur für die Programmierer, die auf der Website arbeiten, zugänglich ist. Nachdem die Anwendung für die Freigabe bereit ist, wird Sie in eine Produktionsumgebung verschoben, in der jeder Benutzer im Internet auf den Standort zugreifen kann. Dieser Bereitstellungs Prozess führt zu einer Reihe von Herausforderungen:

- Eine Produktionsumgebung muss vorhanden sein und ordnungsgemäß eingerichtet werden, bevor eine ASP.NET-Anwendung bereitgestellt werden kann. Darüber hinaus muss die Produktionsumgebung mit den neuesten Sicherheitspatches auf dem neuesten Stand gehalten werden.
- Der richtige Satz von Markup Dateien, Code Dateien und Unterstützungs Dateien muss von der Entwicklungsumgebung in die Produktionsumgebung kopiert werden. Für datengesteuerte Anwendungen ist es möglicherweise erforderlich, dass auch das Datenbankschema und/oder die Daten kopiert werden.
- Es gibt möglicherweise Konfigurations Unterschiede zwischen den beiden Umgebungen. Die in der Entwicklungsumgebung verwendete Datenbank-Verbindungs Zeichenfolge oder der e-Mail-Server unterscheidet sich wahrscheinlich von der Produktionsumgebung. Das Verhalten der Anwendung hängt möglicherweise von der Umgebung ab. Wenn z. b. ein Fehler bei der Entwicklung auftritt, können die Fehlerdetails auf dem Bildschirm angezeigt werden. Wenn jedoch ein Fehler in der Produktion auftritt, sollte stattdessen eine benutzerfreundliche Fehlerseite angezeigt werden, und die Fehlerdetails werden an die Entwickler gesendet.

Um die erste Herausforderung zu umgehen: Einrichtung und Wartung einer Produktionsumgebung: viele Einzelpersonen und Unternehmen lagern ihre Produktionsumgebungen an *Webhostinganbieter*aus. Ein Webhosting-Anbieter ist ein Unternehmen, das die Produktionsumgebung in Ihrem Namen verwaltet. Es gibt unzählige Webhostinganbieter, die jeweils über unterschiedliche Preise und Service Levels verfügen. Tipps zum Auffinden eines solchen Dienstanbieters finden Sie im Abschnitt "Suchen eines webhostanbieters".

Dies ist der erste in einer Reihe von Tutorials, in denen die Schritte zum Bereitstellen einer ASP.NET-Webanwendung in einer Produktionsumgebung beschrieben werden, die von einem Webhost Anbieter verwaltet wird. Im Verlauf dieser Tutorials werden wir Folgendes untersuchen:

- Die Dateien, die für den Webhost Anbieter bereitgestellt werden müssen.
- Tools zur Optimierung des Bereitstellungs Prozesses.
- Bereitstellen einer Datenbank
- Tipps zum Bereitstellen einer Datenbank, die [den SQL-basierten Mitgliedschafts-und Rollen Anbieter](../../older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs.md)verwendet, sowie Möglichkeiten, das Website-Verwaltungs Tool in einer Produktionsumgebung zu imitieren.
- Strategien für eine reibungslose Aktualisierung der Datenbank in der Produktionsumgebung mit während der Entwicklung vorgenommenen Änderungen.
- Verfahren zum Protokollieren von Fehlern, die in der Produktionsumgebung auftreten, und zum Benachrichtigen von Entwicklern, wenn ein Fehler auftritt.

Diese Lernprogramme sind so konzipiert, dass Sie präzise sind und Schritt-für-Schritt-Anleitungen mit vielen Screenshots bereitgestellt werden, damit Sie den Prozess visuell durchlaufen können. Dieses erste Tutorial bietet einen Überblick über den ASP.net-Bereitstellungs Prozess und eine Anleitung zum Suchen eines webhostinganbieters. Erste Schritte

## <a name="an-overview-of-the-aspnet-deployment-process"></a>Übersicht über den ASP.net-Bereitstellungs Prozess

Kurz gesagt: die Bereitstellung einer ASP.NET-Anwendung umfasst die folgenden drei Schritte:

1. Konfigurieren Sie die Webanwendung, den Webserver und die Datenbank in der Produktionsumgebung.
2. Synchronisieren Sie die ASP.NET-Seiten, Code Dateien, die Assemblys im Ordner "`Bin`" und HTML-bezogene Unterstützungs Dateien wie CSS-und JavaScript-Dateien.
3. Synchronisieren Sie das Datenbankschema und/oder die Daten.

Die Konfigurationsinformationen für eine Webanwendung befinden sich in der Regel in der `Web.config`-Datei und umfassen Daten bankverbindungs Zeichenfolgen, Fehler Behandlungs Kriterien, URL-Umschreib Regeln und e-Mail-Server Informationen. Häufig unterscheiden sich diese Informationen für eine Anwendung in der Entwicklung im Vergleich zur gleichen Anwendung in der Produktion. Wenn Sie beispielsweise eine Anwendung entwickeln, empfiehlt es sich, eine Entwicklungs Datenbank zu verwenden, sodass Sie keine Tests für die Produktionsdatenbank durchführen. Folglich unterscheiden sich die Daten bankverbindungs Zeichenfolgen in der Regel zwischen Entwicklungs-und Produktionsanwendungen. Aufgrund dieser Unterschiede besteht ein Teil der Bereitstellung darin, Änderungen an den Konfigurationsinformationen der Webanwendung vorzunehmen.

Zusätzlich zu den Änderungen an der Konfiguration von Webanwendungen kann Schritt 1 auch eine Konfiguration für den Webserver und die Datenbank beinhalten. Wenn z. b. eine ASP.NET-Seite Dateien aus einem Verzeichnis auf dem Webserver erstellt oder löscht, muss der Webserver so konfiguriert werden, dass er diese Dateisystem Änderungen zulässt. Entsprechend sind möglicherweise Berechtigungs-oder Authentifizierungs Einstellungen vorhanden, die an der Datenbank vorgenommen werden müssen.

Schritt 2 umfasst die Synchronisierung der Gruppe der wichtigen ASP.NET-Seiten und Unterstützungs Dateien zwischen den Entwicklungs-und Produktionsumgebungen. Der jeweilige Satz von ASP.NET-bezogenen Dateien, die zwischen den beiden Umgebungen synchronisiert werden müssen, hängt vom Typ des Projekts ab, das Sie in Visual Studio erstellt haben. im nächsten Tutorial wird erläutert, <em>[welche Dateien bereitgestellt werden müssen](determining-what-files-need-to-be-deployed-vb.md)</em>. Das dritte und vierte Lernprogramm: bereitstellen <em>[Ihrer Site mithilfe von FTP](deploying-your-site-using-an-ftp-client-vb.md)</em>und bereitstellen <em>[Ihrer Website mithilfe von Visual Studio](deploying-your-site-using-visual-studio-vb.md)</em> : untersuchen Sie verschiedene Tools und Techniken zum Synchronisieren dieser Dateien.

Wenn Sie datengesteuerte Anwendungen entwickeln, werden in der Regel zwei Datenbanken verwendet: einer für die Entwicklung und einer für die Produktion. Während der Entwicklung kann das Schema der Entwicklungs Datenbank so geändert werden, dass neue Tabellen, Spalten, gespeicherte Prozeduren und Trigger eingeschlossen werden, oder es kann geändert werden, um vorhandene Datenbankobjekte zu entfernen oder umzubenennen. Zwischen dem Zeitpunkt, zu dem diese Änderungen vorgenommen werden, und dem Zeitpunkt, zu dem die Anwendung in der Produktionsumgebung bereitgestellt wird, sind die Entwicklungs-und Produktionsdatenbanken nicht synchron. Diese Asynchronität muss während des Bereitstellungs Prozesses korrigiert werden. Diese Herausforderungen werden in zukünftigen Tutorials untersucht.

## <a name="finding-a-web-host-provider"></a>Suchen eines Webhost Anbieters

ASP.NET-Anwendungen können auf allen Webservern bereitgestellt werden, auf denen die .NET Framework und Internetinformationsdienste (IIS) installiert sind. Sie können einen Standort von Ihrem PC aus hosten, vorausgesetzt, Sie hatten eine Breitbandverbindung mit dem Internet und wissen, wie Sie den Router konfigurieren, um eingehende Webanforderungen zuzulassen. Sie können auch einen Standort von einem Computer in einem Intranet hosten, so wie viele Unternehmen dies tun. Der Schwerpunkt dieser Tutorials besteht jedoch darin, Ihre Website mit einem Webhostinganbieter zu hosten.

> [!NOTE]
> [IIS](https://www.iis.net/) ist der von Microsoft Unternehmens-Webserver. Es ist mit den nicht-Home-Editionen von Windows, wie z. b. Windows Server 2008 und bestimmten Editionen von Windows Vista, ausgeliefert. Sie müssen IIS nicht installieren, um ASP.NET-Anwendungen in einer Entwicklungsumgebung bereitzustellen, da Visual Studio den ASP.NET Development-Webserver enthält. Der ASP.NET Development-Webserver akzeptiert jedoch nur lokale Verbindungen und kann daher nicht in einer Produktionsumgebung verwendet werden.

Bevor Sie Ihren Standort auf einem Webhost Anbieter bereitstellen können, müssen Sie zunächst entscheiden, mit welchem Unternehmen das Unternehmen Geschäfte soll. Im Marketplace sind unzählige Webhostingunternehmen vorhanden. bei einer Suche nach "Webhostingunternehmen" werden mehr als 5 Millionen Ergebnisse zurückgegeben. Wie finden Sie die richtige für Sie? Ihre bevorzugte Suchmaschine ist ein guter Ausgangspunkt, ebenso wie Websites wie [tophosts](http://www.tophosts.com/) und [hostcritique](http://www.hostcritique.net/), die verschiedene Hostingdienste vergleichen und vergleichen. Ich empfehle Ihnen auch, ihre Kollegen und Kollegen bei allen Empfehlungen aufzufordern. Sie können auch in den [ASP.net-Foren](https://forums.asp.net/)auf der Seite " [Hosting Open Forum](https://forums.asp.net/158.aspx) " nach Empfehlungen Fragen.

Webhostingunternehmen bieten in der Regel freigegebene hostingpläne und dedizierte hostingpläne Beim gemeinsamen Hosten eines einzelnen Webservers werden Dutzende von unterschiedlichen Websites gehostet. Mit dediziertem Hosting leasen Sie einen Computer von dem Unternehmen, das Ihren Standort und ihren Standort allein bedient. Ein frei gegebener Hostingplan kann Unterstützung für ASP.NET Seiten, die Möglichkeit zur Arbeit mit Microsoft Access-Datenbanken, 5 GB Speicherplatz und 100 GB monatlicher Bandbreiten Verkehr für $9,95 pro Monat umfassen. Ein anderer gemeinsamer Hostingplan kann Unterstützung für ASP.NET-Seiten, Zugriff auf den Microsoft SQL Server 2008-Daten Bank Server, 10 GB Speicherplatz und 250 GB monatlicher Bandbreiten Verkehr für $19,95 pro Monat umfassen. Dedizierte hostingpläne sind in der Regel viel teurer, was mehrere hundert Dollar pro Monat kostet, aber eine bessere Leistung und mehr Kontrolle bietet als freigegebene Hostingoptionen. Der Plan, den Sie auswählen, hängt von Ihrem Budget, von der Menge an Datenverkehr Ihrer Website und den Features ab, die Sie voraussichtlich benötigen.

Zwei wichtige Überlegungen bei der Auswahl eines Webhost Anbieters sind Customer Service und Quality of Service. Wenn Sie eine Frage oder ein Konfigurationsproblem haben, wie lange dauert es, bis Sie eine Antwort erhalten haben, wenn Sie Ihr Problem an den Helpdesk des Webhosts übermitteln? Wie zuverlässig sind die Dienste des Unternehmens? Gibt es häufig Daten Bankausfälle? Wie oft wird der e-Mail-Server offline geschaltet? Sie können ein Unternehmen jederzeit bitten, ausführliche Informationen zur Betriebszeit bereitzustellen und sich über die Kundendienst Richtlinie zu informieren. eine weitere Möglichkeit besteht jedoch darin, das Feedback aktueller und früherer Kunden anzufordern, die Sie über Onlineforen, Newsgroups und e-Mail-listingdienste durchführen können. .

> [!NOTE]
> Einige Webhostingunternehmen konzentrieren sich auf einen bestimmten Technologie Stapel, wie z. b. .net oder [Lamp](http://en.wikipedia.org/wiki/LAMP_stack) (**L** Inux, **a** pche, **M** ysql und **P** HP). Stellen Sie daher sicher, dass das Unternehmen, das Sie auswählen, ASP.NET-Anwendungen hostet. Stellen Sie außerdem sicher, dass Sie die Version von ASP.NET unterstützen, die Sie zum Erstellen der Anwendung verwenden. Wenn Sie eine datengesteuerte Anwendung erstellen, stellen Sie sicher, dass der Webhost denselben Datenbankserver und die gleiche Version anbietet, die Sie verwenden.

## <a name="summary"></a>Zusammenfassung

ASP.NET Webanwendungen werden in der Regel in einer lokalen Entwicklungsumgebung entworfen, erstellt und getestet. Sobald eine Version für die Freigabe bereit ist, wird Sie in eine Produktionsumgebung verschoben. Obwohl es möglich ist, ASP.NET Websites auf Ihrem eigenen Computer oder auf Servern innerhalb Ihres Unternehmens zu hosten, haben viele Unternehmen und Einzelpersonen die Möglichkeit, Ihr Hosting an einen Webhostinganbieter auszulagern.

In dieser tutorialreihe werden die Schritte zum Bereitstellen einer ASP.NET-Anwendung für einen Webhost Anbieter untersucht. In diesem Tutorial wurde eine allgemeine Übersicht über den ASP.net-Bereitstellungs Prozess bereitgestellt, und es wurden Tipps zum Auffinden eines passenden Webhost Anbieters bereitgestellt. Im nächsten Tutorial wird untersucht, welche ASP.NET-bezogenen Dateien beim Bereitstellen Ihrer Website in die Produktionsumgebung kopiert werden müssen.

Fröhliche Programmierung!

### <a name="special-thanks-to"></a>Besonders vielen Dank...

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Lead Reviewer für dieses Tutorial war Teresa Murphy. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)ablegen.

> [!div class="step-by-step"]
> [Zurück](users-and-roles-on-the-production-website-cs.md)
> [Weiter](determining-what-files-need-to-be-deployed-vb.md)
