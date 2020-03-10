---
uid: whitepapers/denied-access-to-iis-directories
title: ASP.net verweigerten Zugriff auf IIS-Verzeichnisse | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Whitepaper wird beschrieben, was Sie tun müssen, wenn eine Anforderung an Ihre ASP.NET-Anwendung den Fehler "der Zugriff auf das directname-Verzeichnis wurde verweigert" zurückgegeben wird. Fehler bei s...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: 3cb27b8a-354f-4332-bfe0-232b13bbf8aa
msc.legacyurl: /whitepapers/denied-access-to-iis-directories
msc.type: content
ms.openlocfilehash: a3a53aa88abbe1bcaaea7d691406800c8f9b988b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78518571"
---
# <a name="aspnet-denied-access-to-iis-directories"></a>ASP.NET – Verweigerung des Zugriffs auf IIS-Verzeichnisse

> In diesem Whitepaper wird beschrieben, was Sie tun müssen, wenn eine Anforderung an Ihre ASP.NET-Anwendung den Fehler "der Zugriff auf das *directname* -Verzeichnis wurde verweigert" zurückgegeben wird. Fehler beim Starten der Überwachung von Verzeichnisänderungen. "
> 
> Gilt für ASP.NET 1,0 und ASP.NET 1,1.

ASP.net V1 RTM wird nun mit einem Windows-Konto mit weniger Berechtigungen ausgeführt, das als "ASPNET"-Konto auf einem lokalen Computer registriert ist.

Bei einigen gesperrten Systemen verfügt dieses Konto möglicherweise nicht standardmäßig über Lese Sicherheits Zugriff auf die Inhaltsverzeichnisse einer Website, das Stammverzeichnis der Anwendung oder das Stammverzeichnis der Website. In diesem Fall erhalten Sie den folgenden Fehler, wenn Sie Seiten von einer bestimmten Webanwendung anfordern:

![](denied-access-to-iis-directories/_static/image1.jpg)

Um dieses Problem zu beheben, müssen Sie die Sicherheits Berechtigungen für die entsprechenden Verzeichnisse ändern.

ASP.net erfordert insbesondere Lese-, Ausführungs-und Listen Zugriff für das ASPNET-Konto für das Website Stammverzeichnis (z. b. "c:\inetpub\wwwroot" oder ein beliebiges alternatives Website Verzeichnis, das Sie möglicherweise in IIS konfiguriert haben), das Inhaltsverzeichnis und das Stammverzeichnis der Anwendung. um Änderungen an Konfigurationsdateien zu überwachen. Der Anwendungs Stamm entspricht dem Ordner Pfad, der dem virtuellen Anwendungsverzeichnis im IIS-Verwaltungs Tool (inetmgr) zugeordnet ist.

Sehen Sie sich beispielsweise die folgende Anwendungs Hierarchie unter dem Ordner "wwwroot" an.

`C:\inetpub\wwwroot\myapp\default.aspx`

In diesem Beispiel benötigt das ASPNET-Konto die oben definierten Leseberechtigungen für den Inhalt im Verzeichnis "MyApp" und im Verzeichnis "wwwroot". Eine einzelne geerbte ACL für den Stamm Ordner kann optional auch für beide Verzeichnisse verwendet werden, wenn Sie sich in der Liste befinden.

Um Berechtigungen zu einem Verzeichnis hinzuzufügen, führen Sie die folgenden Schritte aus:

- Navigieren Sie im Windows-Explorer zum Verzeichnis.
- Klicken Sie mit der rechten Maustaste auf den Verzeichnis Ordner, und wählen Sie "Eigenschaften
- Navigieren Sie im Eigenschaften Dialogfeld zur Registerkarte "Sicherheit".
- Klicken Sie auf die Schaltfläche "hinzufügen", und geben Sie den Computernamen gefolgt vom Namen des ASPNET-Kontos ein. Geben Sie beispielsweise auf einem Computer mit dem Namen "WebDev" webdev\aspnet ein, und drücken Sie "OK".
- Stellen Sie sicher, dass das ASPNET-Konto die Kontrollkästchen "lesen &amp; ausführen", "Ordnerinhalte auflisten" und "lesen" aktiviert ist.
- OK, um das Dialogfeld zu schließen und die Änderungen zu speichern.

![](denied-access-to-iis-directories/_static/image2.jpg)

Wenn gewünscht, können diese Änderungen mithilfe von Skripts oder dem Tool "Cacls. exe", das mit Windows ausgeliefert wird, automatisiert werden. Weitere Informationen zum ASPNET-Konto finden Sie im [Dokument](https://go.microsoft.com/fwlink/?LinkId=5828)mit häufig gestellten Fragen.

Wenn eine bestimmte Webanwendung Schreib-oder Änderungs Berechtigungen für einen bestimmten Ordner oder eine bestimmte Datei hat, kann dies durch Befolgen desselben Verfahrens und Überprüfen der Kontrollkästchen "schreiben" und/oder "ändern" erfolgen.

Auf Computern, die allen Benutzern oder der Benutzergruppe Lesezugriff auf diese Verzeichnisse (Standardkonfiguration) gewähren, werden keine Probleme gefunden, und die oben aufgeführten Schritte sind nicht erforderlich.
