---
uid: whitepapers/aspnet-and-iis6
title: Ausführen von ASP.NET 1,1 mit IIS 6,0 | Microsoft-Dokumentation
author: rick-anderson
description: Obwohl Windows Server 2003 sowohl IIS 6,0 als auch ASP.NET 1,1 enthält, sind diese komponentenstandard mäßig deaktiviert. In diesem Whitepaper wird beschrieben, wie Sie IIS 6,0 aktivieren...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: 5a5537bf-2aaa-49e7-839f-9e6522b829d8
msc.legacyurl: /whitepapers/aspnet-and-iis6
msc.type: content
ms.openlocfilehash: 2e7812f34481afe9a71927c0d9ba2a9abc9632e4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419847"
---
# <a name="running-aspnet-11-with-iis-60"></a>Ausführen von ASP.NET 1.1 mit IIS 6.0

> Obwohl Windows Server 2003 sowohl IIS 6,0 als auch ASP.NET 1,1 enthält, sind diese komponentenstandard mäßig deaktiviert. Dieses Whitepaper beschreibt die Aktivierung von IIS 6,0 und ASP.NET 1,1 und empfiehlt verschiedene Konfigurationseinstellungen, um die optimale Leistung von IIS und ASP.net zu erzielen.
> 
> Gilt für ASP.NET 1,1 und IIS 6,0.

ASP.NET 1,1 wird mit Windows Server 2003 ausgeliefert, das auch die neueste Version von Internet Information Server (IIS), Version 6,0, umfasst. IIS 6,0 und ASP.NET 1,1 sind so konzipiert, dass Sie nahtlos und ASP.net jetzt standardmäßig das neue IIS 6,0-workerprozessmodell verwendet.

## <a name="aspnet-11-is-not-installed-by-default"></a>ASP.NET 1,1 wird nicht standardmäßig installiert.

Im Gegensatz zu früheren Versionen der Server Betriebssysteme von Microsoft ist IIS (Internet Information Server) standardmäßig nicht aktiviert. und ist nicht ASP.NET 1,1. Es gibt zwei Optionen zum Aktivieren von IIS:

### <a name="enabling-iis-option-1---configure-your-server-wizard"></a>Aktivieren von IIS, Option #1-Konfigurieren des Server-Assistenten

Windows Server 2003 enthält einen neuen Server "Server konfigurieren", mit dem Sie den Server im gewünschten Modus ordnungsgemäß konfigurieren können.

Um den Assistenten zu starten, müssen Sie als Administrator angemeldet sein, um den Assistenten auszuführen: Programme | Verwaltungs Tools, und wählen Sie "Server konfigurieren" aus.

Nachdem Sie die Option ausgewählt haben, wird der Startbildschirm des Assistenten zum Konfigurieren des Servers angezeigt:

![](aspnet-and-iis6/_static/image1.jpg)

Klicken Sie auf "weiter &gt;":

![](aspnet-and-iis6/_static/image2.jpg)

Klicken Sie auf "weiter &gt;".

![](aspnet-and-iis6/_static/image3.jpg)

Auf diesem Bildschirm müssen Sie den Anwendungsserver (IIS, ASP.net) als Optionen für die Konfiguration auswählen.

Klicken Sie auf "weiter &gt;".

![](aspnet-and-iis6/_static/image4.jpg)

Nachdem Sie die Konfiguration des Servers als Anwendungsserver ausgewählt haben, wird dieser Bildschirm angezeigt, in dem Sie dazu aufgefordert werden, welche zusätzlichen Funktionen installiert werden sollen. Keine der beiden Optionen ist standardmäßig ausgewählt. Um ASP.NET automatisch zu aktivieren, müssen Sie "ASP aktivieren" auswählen. NET '.

Klicken Sie auf "weiter &gt;".

![](aspnet-and-iis6/_static/image5.jpg)

Auf diesem Bildschirm wird angezeigt, welche Optionen installiert werden sollen.

Klicken Sie auf "weiter &gt;".

![](aspnet-and-iis6/_static/image6.jpg)

Dieser Bildschirm wird angezeigt, während die von Ihnen ausgewählten Optionen installiert werden. Es ist normal, dass andere Dialogfelder angezeigt werden, wenn Dienste installiert werden. Sie werden möglicherweise zusätzlich zur Eingabe des Speicher Orts der Windows 2003-Server-Installations-CD aufgefordert.

Klicken Sie nach Abschluss des Vorgangs auf ' weiter &gt;'.

![](aspnet-and-iis6/_static/image7.jpg)

Klicken Sie auf "Fertigstellen"-Windows Server 2003 ist nun für die Unterstützung von IIS 6,0 und ASP.NET 1,1 konfiguriert.

### <a name="enabling-iis-option-2---manually-configuring-iis-and-aspnet"></a>Aktivieren von IIS, Option #2-Manuelles Konfigurieren von IIS und ASP.net

Wenn Sie den Serverkonfigurations-Assistenten nicht verwenden möchten, können Sie optional IIS 6,0 und ASP.NET 1,1 mithilfe von "Software" in der Systemsteuerung installieren.

Öffnen Sie zunächst die Systemsteuerung:

![](aspnet-and-iis6/_static/image8.jpg)

Klicken Sie anschließend auf "Windows-Komponenten hinzufügen/entfernen", um den Assistenten für Windows-Komponenten zu öffnen:

![](aspnet-and-iis6/_static/image9.jpg)

Markieren Sie "Anwendungs Server", und klicken Sie darauf, und klicken Sie dann auf "Details?". gedrückt

![](aspnet-and-iis6/_static/image10.jpg)

Um ASP.net zu installieren, aktivieren Sie "ASP. NET '.

Klicken Sie auf "OK", um zum Assistenten für Windows-Komponenten zurückzukehren. Klicken Sie im Assistenten für Windows-Komponenten auf "weiter &gt;", um die Installation zu starten:

![](aspnet-and-iis6/_static/image11.jpg)

Es ist normal, dass andere Dialogfelder angezeigt werden, wenn Dienste installiert werden. Sie werden möglicherweise zusätzlich zur Eingabe des Speicher Orts der Windows 2003-Server-Installations-CD aufgefordert.

Nach Abschluss der Installation wird der letzte Bildschirm des Assistenten für Windows-Komponenten angezeigt:

![](aspnet-and-iis6/_static/image12.jpg)

IIS 6,0 und ASP.NET 1,1 sind nun konfiguriert und verfügbar.

## <a name="recommended-settings"></a>Empfohlene Einstellungen

Beim Ausführen von ASP.NET 1,1 mit IIS 6,0 gibt es mehrere Konfigurationseinstellungen, die empfohlen werden, um die optimale Leistung von ASP.net zu erzielen:

- Konfigurieren von Arbeitsspeicher Limits für Arbeitsprozesse
- Konfigurieren der Wiederverwendung von Arbeitsprozessen

### <a name="configuring-worker-process-memory-limits"></a>Konfigurieren von Arbeitsspeicher Limits für Arbeitsprozesse

Standardmäßig wird von IIS 6,0 keine Beschränkung für die Menge an Arbeitsspeicher festgelegt, die von IIS verwendet werden darf. ASP. Die Cache-Funktion des Netzes basiert auf einer Beschränkung des Arbeitsspeichers, sodass der Cache nicht verwendete Elemente proaktiv aus dem Arbeitsspeicher entfernen kann.

Es wird empfohlen, dass Sie die Speicher Wiederverwendungs Funktion von IIS 6,0 konfigurieren. So konfigurieren Sie diesen geöffneten Internetinformationsdienste-Manager (Start | Programme | Verwaltungs Tools | Internetinformationsdienste). Nachdem Sie geöffnet haben, erweitern Sie den Ordner "Anwendungs Pools":

Für jeden Anwendungs Pool:

![](aspnet-and-iis6/_static/image13.jpg)

1. Klicken Sie mit der rechten Maustaste auf den Anwendungs Pool, z. b. "DefaultAppPool", und wählen Sie "Eigenschaften" aus:

![](aspnet-and-iis6/_static/image14.jpg)

2. Aktivieren Sie als nächstes die Speicher Wiederverwendung, indem Sie entweder auf "Maximum used Memory (in Megabyte):" klicken. Der Wert darf nicht größer sein als der physische (nicht virtuelle) Arbeitsspeicher auf dem Server, eine gute Näherung ist 60% des physischen Speichers, d. h. für einen Server mit 512 MB physischem Arbeitsspeicher wählen Sie 310 aus. Es wird auch empfohlen, bei Verwendung eines 2-GB-Adressraums maximal 800mb zu verwenden. Wenn der Speicher Adressraum des Servers 3 GB beträgt, kann das maximale Arbeitsspeicher Limit für den Arbeitsprozess maximal 1, 800 MB betragen:

![](aspnet-and-iis6/_static/image15.jpg)

Klicken Sie auf "übernehmen" und "OK", um das Eigenschaften Dialogfeld zu schließen. Wiederholen Sie diesen Schritt für alle verfügbaren Anwendungs Pools.

### <a name="configuring-worker-recycling"></a>Konfigurieren von workerwieder

IIS 6,0 ist standardmäßig so konfiguriert, dass der Arbeitsprozess alle 29 Stunden wieder verwendet wird. Dies ist ein wenig aggressiv für eine Anwendung, die ASP.NET ausgeführt wird. es wird empfohlen, die automatische Wiederverwendung des Arbeitsprozesses zu deaktivieren.

Zum Deaktivieren der automatischen Wiederverwendung des Arbeitsprozesses öffnen Sie zunächst Internetinformationsdienste-Manager (Start | Programme | Verwaltungs Tools | Internetinformationsdienste). Nachdem Sie geöffnet haben, erweitern Sie den Ordner "Anwendungs Pools":

![](aspnet-and-iis6/_static/image16.jpg)

Für jeden Anwendungs Pool:

1. Klicken Sie mit der rechten Maustaste auf den Anwendungs Pool, z. b. "DefaultAppPool", und wählen Sie "Eigenschaften" aus:

![](aspnet-and-iis6/_static/image17.jpg)

2. Deaktivieren Sie die Option "Arbeitsprozess wieder verwenden (in Minuten):":

![](aspnet-and-iis6/_static/image18.jpg)

Klicken Sie auf "übernehmen" und "OK", um das Eigenschaften Dialogfeld zu schließen. Wiederholen Sie diesen Schritt für alle verfügbaren Anwendungs Pools.

## <a name="granting-write-access-to-the-file-system"></a>Gewähren von Schreibzugriff auf das Dateisystem

Wenn Ihre Anwendung Schreibzugriff auf das Dateisystem benötigt und Sie NTFS verwenden, müssen Sie eine Access Control Liste (ACL) für den Ordner oder die Datei ändern, um ASP.net Zugriff zu gewähren.

Wenn Sie z. b. dem ersten geöffneten Explorer "c:\inetpub\wwwroot" ASP.net Schreibzugriff gewähren möchten, navigieren Sie zu dem Verzeichnis:

![](aspnet-and-iis6/_static/image19.jpg)

Klicken Sie dann mit der rechten Maustaste auf das Verzeichnis, z. b. "wwwroot", und wählen Sie Eigenschaften aus. Wählen Sie nach dem Öffnen des Dialog Felds Eigenschaften die Registerkarte "Sicherheit" aus:

![](aspnet-and-iis6/_static/image20.jpg)

Das Verzeichnis "c:\inetpub\wwwroot\" ist ein spezielles Verzeichnis, in dem die spezielle IIS 6,0-Gruppe "IIS\_WPG" bereits lesen &amp; ausführen, Ordner Inhalt auflisten und Leseberechtigungen gewährt wurde. Um jedoch Schreibberechtigungen zu erteilen, müssen Sie auf das Kontrollkästchen Zulassen für Write (schreiben) klicken:

![](aspnet-and-iis6/_static/image21.jpg)

IIS 6,0 verfügt jetzt über die Schreib Berechtigung für diesen Ordner. Gehen Sie folgendermaßen vor, um Schreibberechtigungen für andere Ordner zu erteilen: Beachten Sie, dass Sie möglicherweise die IIS-\_WPG-Gruppe hinzufügen müssen, falls diese nicht bereits vorhanden ist.

> [!CAUTION]
> Das Erteilen von Schreibberechtigungen für IIS\_WPG ermöglicht allen ASP.NET-Anwendungen das Schreiben in dieses Verzeichnis.

## <a name="supporting-integrated-authentication-with-sql-server"></a>Unterstützung der integrierten Authentifizierung mit SQL Server

Die integrierte Authentifizierung ermöglicht SQL Server die Windows NT-Authentifizierung zum Überprüfen von SQL Server Anmeldekonten zu nutzen. Dadurch kann der Benutzer den Standard-SQL Server Anmeldeprozess umgehen. Bei diesem Ansatz kann ein Netzwerk Benutzer auf eine SQL Server Datenbank zugreifen, ohne eine separate Anmelde-ID oder ein Kennwort anzugeben, da SQL Server die Benutzer-und Kenn Wort Informationen aus dem Windows NT-Netzwerk Sicherheitsprozess abruft.

Die Auswahl integrierter Authentifizierung für ASP.NET-Anwendungen ist eine gute Wahl, da in der Verbindungs Zeichenfolge für Ihre Anwendung niemals Anmelde Informationen gespeichert werden. Die Verbindungs Zeichenfolge, die zum Herstellen einer Verbindung mit SQL verwendet wird, sieht stattdessen wie folgt

`"server=localhost; database=Northwind;Trusted_Connection=true"`

Diese Verbindungs Zeichenfolge weist SQL Server an, die Windows-Anmelde Informationen der Anwendung zu verwenden, die auf SQL Server zuzugreifen versucht. Im Fall von ASP.NET/IIS 6 wäre dies ein Konto in der IIS-\_WPG-Gruppe.

Um die integrierte Authentifizierung zwischen SQL Server und ASP.net zu aktivieren, müssen Sie zunächst sicherstellen, dass SQL Server entweder für die integrierte Authentifizierung oder die Authentifizierung im gemischten Modus konfiguriert ist. Überprüfen Sie hierzu den DBA, um dies zu bestimmen. Wenn sich SQL Server in einem dieser beiden Modi befindet, können Sie die integrierte Authentifizierung verwenden.

Öffnen Sie SQL Server Enterprise-Manager (Start | Programme | Microsoft SQL Server | Enterprise Manager), wählen Sie den entsprechenden Server aus, und erweitern Sie den Ordner Sicherheit:

![](aspnet-and-iis6/_static/image22.jpg)

Wenn die Gruppe "builtint\iis\_WPG ' nicht aufgeführt ist, klicken Sie mit der rechten Maustaste auf Anmeldungen, und wählen Sie" neu Login"aus:

![](aspnet-and-iis6/_static/image23.jpg)

Geben Sie im Textfeld "Name:" entweder "[Server/Domänen Name] \IIS\_WPG" ein, oder klicken Sie auf die Schaltfläche mit den Auslassungs Punkten, um die Benutzer-/Gruppenauswahl von Windows NT zu öffnen:

![](aspnet-and-iis6/_static/image24.jpg)

Wählen Sie die IIS-\_WPG-Gruppe des aktuellen Computers aus, und klicken Sie auf "hinzufügen", um die Auswahl zu schließen.

Anschließend müssen Sie auch die Standarddatenbank und die Berechtigungen für den Zugriff auf die Datenbank festlegen. Um die Standarddatenbank festzulegen, wählen Sie in der Dropdown Liste aus, z. b. unter Northwind ist ausgewählt:

![](aspnet-and-iis6/_static/image25.jpg)

Klicken Sie anschließend auf die Registerkarte Datenbankzugriff:

![](aspnet-and-iis6/_static/image26.jpg)

Klicken Sie auf das Kontrollkästchen Zulassen für jede Datenbank, für die Sie den Zugriff zulassen möchten. Sie müssen auch Daten bankrollen auswählen und DB überprüfen\_der Besitzer sicherstellt, dass Ihre Anmeldung über alle erforderlichen Berechtigungen zum Verwalten und Verwenden der ausgewählten Datenbank verfügt.

Klicken Sie auf OK, um das Eigenschafts Dialogfeld Ihre ASP.NET-Anwendung ist nun so konfiguriert, dass die integrierte SQL Server Authentifizierung unterstützt wird.

## <a name="dont-run-aspnet-10-in-iis-60-native-mode"></a>ASP.NET 1,0 nicht im einheitlichen Modus von IIS 6,0 ausführen

ASP.NET 1,0 unter IIS 6,0 wird nur im IIS 5-Kompatibilitätsmodus unterstützt.

Öffnen Sie Internetdienste-Manager, und klicken Sie mit der rechten Maustaste auf Websites, und wählen Sie Eigenschaften aus, um ASP.NET 1,0 für die im IIS 5,0-Kompatibilitätsmodus

![](aspnet-and-iis6/_static/image27.jpg)

Wechseln Sie zur Registerkarte Dienst, und überprüfen Sie? WWW-Dienst im IIS 5,0-Isolations Modus ausführen?:

![](aspnet-and-iis6/_static/image28.jpg)
