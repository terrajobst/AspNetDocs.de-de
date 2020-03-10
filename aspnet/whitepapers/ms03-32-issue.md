---
uid: whitepapers/ms03-32-issue
title: Fehlerbehebung für den Fehler "Server Anwendung nicht verfügbar" nach dem Anwenden des Sicherheitsupdates für IE | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Dokument wird der Patch beschrieben, mit dem ein Problem mit dem MS03-32-Sicherheits Update für Internet Explorer behoben wird, das sich auf ASP.NET 1,0-Anwendungen auswirkt, die auf Wi
ms.author: riande
ms.date: 02/10/2010
ms.assetid: 1365eebb-bdf7-4a05-8d18-7f200531be55
msc.legacyurl: /whitepapers/ms03-32-issue
msc.type: content
ms.openlocfilehash: e0b6776cbfe22e341ac7105f03daac5074b480fc
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78463455"
---
# <a name="fix-for-server-application-unavailable-error-after-applying-security-update-for-ie"></a>Fehlerbehebung für den Fehler „Die Serveranwendung ist nicht verfügbar“ nach Anwendung des Sicherheitsupdates für IE

> In diesem Dokument wird der Patch beschrieben, mit dem ein Problem mit dem MS03-32-Sicherheits Update für Internet Explorer behoben wird, das sich auf ASP.NET 1,0-Anwendungen unter Windows XP Professional auswirkt.
> 
> Gilt für ASP.NET 1,0 und Windows XP Professional.

Microsoft hat ein Problem mit dem MS03-32-Sicherheits Update für Internet Explorer-Sicherheitspatches und der Ausführung von ASP.NET 1,0 unter Windows XP identifiziert. Dieser Patch kann manuell oder durch Abrufen aktueller kritischer Updates von der Windows Update Site installiert werden.

Das Symptom dieses Problems ist, dass nach der Installation des Patches auf einem Computer mit Windows XP alle Anforderungen an ASP.NET-Anwendungen, die auf dem lokalen IIS 5,1-Webserver ausgeführt werden, zu einer Fehlermeldung mit dem Hinweis "Serveranwendung nicht verfügbar" führen. Anforderungen an Remoteweb Server sind nicht betroffen.

Dieses Problem wirkt sich nur auf Installationen aus, die unter Windows XP ASP.NET 1,0 ausgeführt werden. Dies hat keine Auswirkungen auf Computer, auf denen Windows 2000 oder Windows Server 2003 ausgeführt wird. Dies hat auch keine Auswirkungen auf Computer, auf denen Windows XP mit installierter ASP.NET 1,1 installiert ist.

Beachten Sie, dass es **sich** bei diesem Problem nicht um einen Sicherheitsfehler mit ASP.net handelt. Sie **öffnet keine** böswilligen Angriffe auf eine ASP.NET-Anwendung oder einen-Server. Stattdessen handelt es sich um einen reinen funktionalen Bug, der durch den Patch selbst verursacht wird.

Wir arbeiten hart an einer permanenten Lösung für dieses Problem. In der Zwischenzeit können Sie die folgende Batchdatei als Problem Umgehung für das Problem ausführen. Die Batchdatei führt Folgendes aus:

1. Stoppt die IIS-und ASP.net-Zustands Dienste.
2. Hiermit wird das ASPNET-Konto mit einem bekannten temporären Kennwort gelöscht und neu erstellt.
3. Verwendet den Windows `runas`-Befehl zum Starten einer ausführbaren Datei, die ein ASPNET-Benutzerprofil erstellt.
4. Registriert ASP.net erneut. Dadurch wird ein neues zufälliges Kennwort für das Konto erstellt, und die Standardeinstellungen für die ASP.net-Zugriffs Steuerung werden angewendet
5. Startet den IIS-Dienst neu.

Die Batchdatei enthält ein hart codiertes temporäres Kennwort mit dem<strong>Text "1pass\@Word</strong>", das Sie beim Ausführen der Batchdatei zur Eingabe für den runas-Befehl aufgefordert werden. Nachdem der RUNAS-Befehl abgeschlossen wurde, wird das Kennwort des ASPNET-Kontos mit einem starken Zufallswert neu erstellt. Beachten Sie, dass die Batchdatei möglicherweise fehlschlägt, wenn das hart codierte Kennwort nicht den Komplexitäts Anforderungen für Kenn Wörter in Ihrer Umgebung entspricht. Wenn dies der Fall ist, können Sie ihn in einen anderen Wert ändern, der für Ihre Umgebung geeignet ist.

*> [!IMPORTANT]* Wenn Sie benutzerdefinierte Zugriffs Steuerungseinstellungen oder Daten Bank Konto Berechtigungen für das ASPNET-Konto hinzugefügt haben, müssen Sie neu erstellt werden, nachdem diese Batchdatei abgeschlossen wurde. Dies liegt daran, dass beim erneuten Erstellen des Kontos eine neue Sicherheits-ID (SID) erhalten wird.

*> [!IMPORTANT]* Wenn Sie den ASP.NET-Arbeitsprozess mit einem anderen Benutzerkonto als dem ASPNET-Konto ausführen, sollte diese Batchdatei nicht ausgeführt werden. Melden Sie sich stattdessen interaktiv an, oder verwenden Sie den Befehl "runas" mit dem Konto, mit dem ein Benutzerprofil für dieses Konto erstellt wird.

Die Batchdatei ist im folgenden selbst extrahierenden Archiv enthalten. Gehen Sie zur Verwendung wie folgt vor:

1. Sie müssen als Konto mit Administrator Rechten ausgeführt werden.
2. [Herunterladen und Öffnen der selbst extrahierenden ausführbaren Datei](ms03-32-issue/_static/fixup1.exe)
3. Inhalt in c:\ extrahieren
4. Wählen Sie ausführen... aus. Geben Sie im Startmenü ein, und geben Sie `cmd.exe`
5. Geben Sie in den geöffneten Befehls Fenstern `c:\fixup.cmd`ein.
6. Wenn Sie dazu aufgefordert werden, geben Sie <strong>1pass\@Word</strong> als Kennwort ein.
7. Wenn Sie bereits über benutzerdefinierte Zugriffs Steuerungseinstellungen oder Daten Bank Konto Berechtigungen für das ASPNET-Konto verfügen, müssen Sie diese Einstellungen jetzt erneut anwenden.

Viele entschuldigen sich für die Unannehmlichkeiten, die dies verursacht hat. Wir stellen zusätzliche Informationen bereit, sobald Sie verfügbar werden.

In der folgenden Matrix werden die von diesem Problem betroffenen Plattformen und Versionen erläutert.

| .NET Framework | Plattform | Ausgewirkt |
| --- | --- | --- |
| Version 1.0 | Windows 2000 Professional | Nein |
| Version 1.0 | Windows 2000 Server | Nein |
| Version 1.0 | Windows XP Professional | Ja |
| Version 1.0 | Windows Server 2003 | Nein |
| Version 1.0 | Windows XP Home mit Cassini | Nein |
| Version 1.1 | Windows 2000 Professional | Nein |
| Version 1.1 | Windows 2000 Server | Nein |
| Version 1.1 | Windows XP Professional | Nein |
| Version 1.1 | Windows Server 2003 | Nein |
| Version 1.1 | Windows XP Home mit Cassini | Nein |

Vielen Dank   
 Das ASP.net-Team
