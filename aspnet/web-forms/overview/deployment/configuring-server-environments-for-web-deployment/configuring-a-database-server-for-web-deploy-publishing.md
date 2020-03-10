---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing
title: Konfigurieren eines Datenbankservers für die Web deploy Veröffentlichung | Microsoft-Dokumentation
author: jrjlee
description: In diesem Thema wird beschrieben, wie Sie einen SQL Server 2008 R2-Daten Bank Server zur Unterstützung der Webbereitstellung und-Veröffentlichung konfigurieren Die in diesem Thema beschriebenen Aufgaben sind Co...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: e7c447f9-eddf-4bbe-9f18-3326d965d093
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing
msc.type: authoredcontent
ms.openlocfilehash: ade3c1ba1c470092f512436f39b8831458408c2c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78515241"
---
# <a name="configuring-a-database-server-for-web-deploy-publishing"></a>Konfigurieren eines Datenbankservers für die Web Deploy-Veröffentlichung

von [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird beschrieben, wie Sie einen SQL Server 2008 R2-Daten Bank Server zur Unterstützung der Webbereitstellung und-Veröffentlichung konfigurieren
> 
> Die in diesem Thema beschriebenen Aufgaben sind von allen Bereitstellungs Szenarien&#x2014;abhängig. es spielt keine Rolle, ob die Webserver für die Verwendung des Remote-Agent-Diensts des IIS-Webbereitstellungs Tools (Web deploy), des Web deploy Handlers oder der Offline Bereitstellung konfiguriert sind oder die Anwendung auf einem einzelnen Webserver oder einer Serverfarm ausgeführt wird. Die Art und Weise, wie Sie die Datenbank bereitstellen, kann sich je nach Sicherheitsanforderungen und anderen Überlegungen ändern Beispielsweise können Sie die Datenbank mit oder ohne Beispiel Daten bereitstellen, und Sie können Benutzer Rollen Zuordnungen bereitstellen oder Sie nach der Bereitstellung manuell konfigurieren. Die Art und Weise, wie Sie den Datenbankserver konfigurieren, bleibt jedoch unverändert.

Sie müssen keine zusätzlichen Produkte oder Tools installieren, um einen Datenbankserver zur Unterstützung der Webbereitstellung zu konfigurieren. Vorausgesetzt, dass der Datenbankserver und der Webserver auf unterschiedlichen Computern ausgeführt werden, müssen Sie lediglich die folgenden Schritte ausführen:

- Ermöglicht die Kommunikation von SQL Server mithilfe von TCP/IP.
- Ermöglicht die SQL Server von Datenverkehr durch Firewalls.
- Stellen Sie dem Webserver-Computer Konto eine SQL Server-Anmeldung zur Verfügung.
- Ordnen Sie die Anmelde Informationen des Computer Kontos allen erforderlichen Daten bankrollen zu.
- Legen Sie dem Konto, von dem die Bereitstellung ausgeführt wird, eine SQL Server-Anmelde-und Daten Bank Ersteller
- Um Wiederholungs Bereitstellungen zu unterstützen, ordnen Sie die Anmelde Informationen des Bereitstellungs Kontos der Daten Bank Rolle " **DB\_Owner** " zu.

In diesem Thema wird gezeigt, wie Sie die einzelnen Prozeduren ausführen. Bei den Aufgaben und exemplarischen Vorgehensweisen in diesem Thema wird davon ausgegangen, dass Sie mit einer Standard Instanz von SQL Server 2008 R2 auf Windows Server 2008 R2 beginnen. Bevor Sie fortfahren, stellen Sie Folgendes sicher:

- Windows Server 2008 R2 Service Pack 1 und alle verfügbaren Updates werden installiert.
- Der Server ist einer Domäne beigetreten.
- Der Server verfügt über eine statische IP-Adresse.
- SQL Server 2008 R2 Service Pack 1 und alle verfügbaren Updates werden installiert.

Die SQL Server Instanz muss nur die **Datenbank-Engine Services** -Rolle enthalten, die automatisch in jeder SQL Server-Installation enthalten ist. Zur Erleichterung der Konfiguration und Wartung empfiehlt es sich jedoch, die **Verwaltungs Tools – Basic** und **Verwaltungs Tools – Complete** Server-Rollen einzubeziehen.

> [!NOTE]
> Weitere Informationen zum Hinzufügen von Computern zu einer Domäne finden Sie unter [Hinzufügen von Computern zur Domäne und anmelden](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Weitere Informationen zum Konfigurieren statischer IP-Adressen finden Sie unter [Konfigurieren einer statischen IP-Adresse](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx). Weitere Informationen zum Installieren von SQL Server finden Sie unter [Installieren von SQL Server 2008 R2](https://technet.microsoft.com/library/bb500395.aspx).

## <a name="enable-remote-access-to-sql-server"></a>Aktivieren des Remote Zugriffs auf SQL Server

SQL Server verwendet TCP/IP für die Kommunikation mit Remote Computern. Wenn sich der Datenbankserver und der Webserver auf unterschiedlichen Computern befinden, müssen Sie folgende Schritte ausführen:

- Konfigurieren Sie SQL Server Netzwerkeinstellungen, um die Kommunikation über TCP/IP zuzulassen.
- Konfigurieren Sie Hardware-oder Software Firewalls, um TCP-Datenverkehr (und in einigen Fällen UDP-Datenverkehr (User Datagram Protocol) an den von der SQL Server Instanz verwendeten Ports zuzulassen.

Um SQL Server die Kommunikation über TCP/IP zu ermöglichen, verwenden Sie SQL Server-Konfigurations-Manager, um die Netzwerkkonfiguration für die SQL Server Instanz zu ändern.

**So aktivieren Sie SQL Server die Kommunikation mit TCP/IP**

1. Zeigen Sie im Menü **Start** auf **Alle Programme**, klicken Sie auf **Microsoft SQL Server 2008 R2**, klicken Sie auf **Konfigurationstools**, und klicken Sie dann auf **SQL Server-Konfigurations-Manager**.
2. Erweitern Sie im Struktur Ansichts Bereich **SQL Server Netzwerkkonfiguration**, und klicken Sie dann auf **Protokolle für MSSQLSERVER**.

   > [!NOTE]
   > Wenn Sie mehrere Instanzen von SQL Server installiert haben, wird für jede Instanz ein <strong>Protokoll für</strong>das<em>[Instanzname]</em> -Element angezeigt. Sie müssen Netzwerkeinstellungen auf instanzweise konfigurieren.
3. Klicken Sie im Detailfenster mit der rechten Maustaste auf die Zeile **TCP/IP** , und klicken Sie dann auf **aktivieren**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image1.png)
4. Klicken Sie im Dialogfeld **Warnung** auf **OK**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image2.png)
5. Sie müssen den MSSQLServer-Dienst neu starten, damit die neue Netzwerkkonfiguration wirksam wird. Dies können Sie an einer Eingabeaufforderung, in der Dienste-Konsole oder in SQL Server Management Studio. In diesem Verfahren verwenden Sie SQL Server Management Studio.
6. Schließen Sie den SQL Server-Konfigurations-Manager.
7. Zeigen Sie im Menü **Start** auf **Alle Programme**, klicken Sie auf **Microsoft SQL Server 2008 R2**, und klicken Sie dann auf **SQL Server Management Studio**.
8. Geben Sie im Dialogfeld **Verbindung mit Server herstellen** im Feld **Server Name** den Namen des Datenbankservers ein, und klicken Sie dann auf **verbinden**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image3.png)
9. Klicken Sie im **Objekt-Explorer** Bereich mit der rechten Maustaste auf den übergeordneten Server Knoten (z. b. **TESTDB1**), und klicken Sie dann auf **neu starten**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image4.png)
10. Klicken Sie im Dialogfeld **Microsoft SQL Server Management Studio** auf **Ja**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image5.png)
11. Wenn der Dienst neu gestartet wurde, schließen Sie SQL Server Management Studio.

Um SQL Server Datenverkehr über eine Firewall zuzulassen, müssen Sie zunächst wissen, welche Ports von Ihrer SQL Server Instanz verwendet werden. Dies hängt davon ab, wie die SQL Server Instanz erstellt und konfiguriert wurde:

- Eine *Standard Instanz* von SQL Server die Anforderungen an den TCP-Port 1433 abhört (und darauf antwortet).
- Eine *benannte Instanz* von SQL Server die Anforderungen an einen dynamisch zugewiesenen TCP-Port abhört (und darauf antwortet).
- Wenn der SQL Server-Browser-Dienst aktiviert ist, können Clients den Dienst auf UDP-Port 1434 Abfragen, um herauszufinden, welcher TCP-Port für eine bestimmte SQL Server Instanz verwendet werden soll. Dieser Dienst wird jedoch aus Sicherheitsgründen häufig deaktiviert.

Angenommen, Sie verwenden eine Standard Instanz von SQL Server, müssen Sie die Firewall so konfigurieren, dass Datenverkehr zugelassen wird.

| Direction | Von Port | An Port | Porttyp |
| --- | --- | --- | --- |
| Eingehend | Beliebig | 1433 | TCP |
| Ausgehend | 1433 | Beliebig | TCP |

> [!NOTE]
> Aus technischer Sicht verwendet ein Client Computer einen zufällig zugewiesenen TCP-Port zwischen 1024 und 5000, um mit SQL Server zu kommunizieren, und Sie können Ihre Firewallregeln entsprechend einschränken. Weitere Informationen zu SQL Server Ports und Firewalls finden Sie unter [TCP/IP-Portnummern für die Kommunikation mit SQL über eine Firewall](https://go.microsoft.com/?linkid=9805125) und Gewusst [wie: Konfigurieren eines Servers für das lauschen an einem bestimmten TCP-Port (SQL Server-Konfigurations-Manager)](https://msdn.microsoft.com/library/ms177440.aspx).

In den meisten Windows Server-Umgebungen ist es wahrscheinlich, dass Sie die Windows-Firewall auf dem Daten Bank Server konfigurieren müssen. Standardmäßig lässt die Windows-Firewall den gesamten ausgehenden Datenverkehr zu, es sei denn, dies wird von einer Regel Um dem Webserver das Erreichen der Datenbank zu ermöglichen, müssen Sie eine eingehende Regel konfigurieren, die TCP-Datenverkehr an der von der SQL Server Instanz verwendeten Portnummer zulässt. Wenn Sie eine Standard Instanz von SQL Server verwenden, können Sie diese Regel mithilfe des nächsten Verfahrens konfigurieren.

**So konfigurieren Sie die Windows-Firewall für die Kommunikation mit einer Standard SQL Server Instanz**

1. Zeigen Sie auf dem Datenbankserver im **Startmenü** auf **Verwaltung**, und klicken Sie dann auf **Windows-Firewall mit**erweiterter Sicherheit.
2. Klicken Sie im Struktur Ansichts Bereich auf **Eingehende Regeln**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image6.png)
3. Klicken Sie im **Aktions** Bereich unter **Eingehende Regeln**auf **neue Regel**.
4. Wählen Sie im Assistenten für neue eingehende Regeln auf der Seite **Regeltyp** die Option **Port**aus, und klicken Sie dann auf **weiter**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image7.png)
5. Vergewissern Sie sich, dass auf der Seite **Protokoll und Ports** die Option **TCP** ausgewählt ist, und geben Sie im Feld **bestimmte lokale Ports** den Wert **1433**ein, und klicken Sie dann auf **weiter**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image8.png)
6. Lassen Sie auf der Seite Aktion **die Option Verbindung zulassen** aktiviert, und klicken Sie auf **weiter**.
7. Lassen Sie auf der Seite **Profil** die Option **Domäne** ausgewählt, deaktivieren Sie die Kontrollkästchen **Privat** und **öffentlich** , und klicken Sie dann auf **weiter**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image9.png)
8. Geben Sie auf der Seite **Name** einen aussagekräftigen Namen für die Regel ein (z. b. **SQL Server Standard Instanz – Netzwerk Zugriff**), und klicken Sie dann auf **Fertig**stellen.

Weitere Informationen zum Konfigurieren der Windows-Firewall für SQL Server, insbesondere, wenn Sie mit SQL Server über nicht standardmäßige oder dynamische Ports kommunizieren müssen, finden Sie unter Vorgehens [Weise: Konfigurieren einer Windows-Firewall für Datenbank-Engine Zugriff](https://technet.microsoft.com/library/ms175043.aspx).

## <a name="configure-logins-and-database-permissions"></a>Konfigurieren von Anmeldungen und Daten Bank Berechtigungen

Beim Bereitstellen einer Webanwendung für Internetinformationsdienste (IIS) wird die Anwendung mit der Identität des Anwendungs Pools ausgeführt. In einer Domänen Umgebung verwenden Anwendungs Pool Identitäten das Computer Konto des Servers, auf dem Sie ausgeführt werden, um auf Netzwerkressourcen zuzugreifen. Computer Konten haben das Format <em>[Domänen Name]</em><strong>\</strong ><em>[Computername]</em> <strong>$</strong> &#x2014;beispielsweise <strong>fabrikam\testweb1 $</strong>. Damit Ihre Webanwendung über das Netzwerk auf eine Datenbank zugreifen kann, müssen Sie folgende Schritte ausführen:

- Fügen Sie der SQL Server Instanz einen Anmelde Namen für das Computer Konto des Webservers hinzu.
- Ordnen Sie die Anmelde Informationen des Computer Kontos allen erforderlichen Daten bankrollen zu (in der Regel **DB\_DataReader** und **DB\_-DataWriter**).

Wenn die Webanwendung nicht auf einem einzelnen Server, sondern in einer Serverfarm ausgeführt wird, müssen Sie diese Prozeduren für jeden Webserver in der Serverfarm wiederholen.

> [!NOTE]
> Weitere Informationen zu Anwendungs Pool Identitäten und zum Zugreifen auf Netzwerkressourcen finden Sie unter [Anwendungs Pool Identitäten](https://go.microsoft.com/?linkid=9805123).

Sie können diese Aufgaben auf verschiedene Weise angehen. Zum Erstellen der Anmeldung können Sie folgende Aktionen ausführen:

- Erstellen Sie die Anmeldung manuell auf dem Datenbankserver mithilfe von Transact-SQL oder SQL Server Management Studio.
- Verwenden Sie ein SQL Server 2008-Server Projekt in Visual Studio, um die Anmeldung zu erstellen und bereitzustellen.

Eine SQL Server Anmeldung ist ein Objekt auf Server Ebene anstelle eines Objekts auf Datenbankebene, sodass es nicht von der Datenbank abhängig ist, die Sie bereitstellen möchten. Daher können Sie die Anmeldung jederzeit erstellen, und der einfachste Ansatz besteht häufig darin, den Anmelde Namen manuell auf dem Datenbankserver zu erstellen, bevor Sie mit der Bereitstellung von Datenbanken beginnen. Sie können das nächste Verfahren zum Erstellen einer Anmeldung in SQL Server Management Studio verwenden.

**So erstellen Sie eine SQL Server Anmeldung für das Webserver-Computer Konto**

1. Zeigen Sie auf dem Datenbankserver im Menü **Start** auf **Alle Programme**, klicken Sie auf **Microsoft SQL Server 2008 R2**, und klicken Sie dann auf **SQL Server Management Studio**.
2. Geben Sie im Dialogfeld **Verbindung mit Server herstellen** im Feld **Server Name** den Namen des Datenbankservers ein, und klicken Sie dann auf **verbinden**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image10.png)
3. Klicken Sie im **Objekt-Explorer** Bereich mit der rechten Maustaste auf **Sicherheit**, zeigen Sie auf **neu**, und klicken Sie dann auf **Anmelden**.
4. Geben Sie im Dialogfeld **Anmeldung – neu** im Feld **Anmelde Name** den Namen des Computer Kontos des Webservers ein (z. b. **fabrikam\testweb1 $** ).

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image11.png)
5. Klicken Sie auf **OK**.

An diesem Punkt ist der Datenbankserver für die Web deploy Veröffentlichung bereit. Allerdings funktionieren alle Lösungen, die Sie bereitstellen, erst dann, wenn Sie die Computer Kontoanmeldung den erforderlichen Daten bankrollen zuordnen. Die Zuordnung des Anmelde namens zu Daten bankrollen erfordert viel mehr, da Sie erst nach der Bereitstellung der Datenbank Rollen zuordnen können. Sie haben folgende Möglichkeiten, um die Anmeldung des Computer Kontos den erforderlichen Daten bankrollen zuzuordnen:

- Weisen Sie die Daten bankrollen der Anmeldung manuell zu, nachdem Sie die Datenbank zum ersten Mal bereitgestellt haben.
- Verwenden Sie ein Skript nach der Bereitstellung, um der Anmeldung die Daten bankrollen zuzuweisen.

Weitere Informationen zum Automatisieren der Erstellung von Anmeldungen und Daten bankrollen Zuordnungen finden Sie unter Bereitstellen von [Daten bankrollen Mitgliedschaften in Test Umgebungen](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). Alternativ können Sie das nächste Verfahren verwenden, um die Computer Kontoanmeldung manuell den erforderlichen Daten bankrollen zuzuordnen. Beachten Sie, dass Sie dieses Verfahren erst ausführen können, *nachdem* Sie die Datenbank bereitgestellt haben.

**So ordnen Sie der Webserver-Computer Kontoanmeldung Daten bankrollen zu**

1. Öffnen Sie SQL Server Management Studio wie zuvor.
2. Erweitern Sie im **Objekt-Explorer** Bereich den Knoten **Sicherheit** , erweitern Sie den Knoten **Anmeldungen** , und doppelklicken Sie dann auf die Anmelde Informationen des Computer Kontos (z. b. **fabrikam\testweb1 $** ).

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image12.png)
3. Klicken Sie im Dialogfeld **Anmeldungs Eigenschaften** auf **Benutzer Zuordnung**.
4. Wählen Sie in der Liste Benutzer, die **dieser Anmeldung zugeordnet** sind den Namen der Datenbank aus (z. **b. ContactManager**).
5. Wählen Sie in der Liste **Mitgliedschaft in Daten Bank Rolle für:** *[Datenbankname]* die erforderlichen Berechtigungen aus. Wenn Sie die Beispiellösung Contact Manager auswählen, müssen Sie die Rollen **DB\_DataReader** und **DB\_-DataWriter** auswählen.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image13.png)
6. Klicken Sie auf **OK**.

Die manuelle Zuordnung von Daten bankrollen ist für Testumgebungen häufig nicht ausreichend, aber es ist weniger wünschenswert für automatisierte oder One-Click-bereit Stellungen in Staging-oder Produktionsumgebungen. Weitere Informationen zur Automatisierung dieser Art von Aufgaben finden Sie unter Bereitstellen von Skripts nach der Bereitstellung in der Bereitstellung [von Daten bankrollen Mitgliedschaften in Test Umgebungen](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md).

> [!NOTE]
> Weitere Informationen zu Server Projekten und Datenbankprojekten finden Sie unter [Visual Studio 2010 SQL Server-Datenbankprojekte](https://msdn.microsoft.com/library/ff678491.aspx).

## <a name="configure-permissions-for-the-deployment-account"></a>Konfigurieren von Berechtigungen für das Bereitstellungs Konto

Wenn das Konto, das Sie zum Ausführen der Bereitstellung verwenden, kein SQL Server Administrator ist, müssen Sie auch einen Anmelde Namen für dieses Konto erstellen. Das Konto muss ein Mitglied der **dbcreator** -Server Rolle sein oder über entsprechende Berechtigungen verfügen, um die Datenbank erstellen zu können.

> [!NOTE]
> Wenn Sie Web deploy oder VSDBCmd verwenden, um eine Datenbank bereitzustellen, können Sie Windows-Anmelde Informationen oder SQL Server Anmelde Informationen verwenden (wenn die SQL Server Instanz zur Unterstützung der Authentifizierung im gemischten Modus konfiguriert ist). Im nächsten Verfahren wird davon ausgegangen, dass Sie Windows-Anmelde Informationen verwenden möchten, aber es hindert Sie nicht daran, bei der Konfiguration der Bereitstellung einen SQL Server Benutzernamen und ein Kennwort in der Verbindungs Zeichenfolge anzugeben.

**So richten Sie Berechtigungen für das Bereitstellungs Konto ein**

1. Öffnen Sie SQL Server Management Studio wie zuvor.
2. Klicken Sie im **Objekt-Explorer** Bereich mit der rechten Maustaste auf **Sicherheit**, zeigen Sie auf **neu**, und klicken Sie dann auf **Anmelden**.
3. Geben Sie im Dialogfeld **Anmeldung – neu** im Feld **Anmelde Name** den Namen Ihres Bereitstellungs Kontos ein (z. b. **fabrikam\matt**).
4. Klicken Sie im Bereich **Seite auswählen** auf **Server Rollen**.
5. Wählen Sie **dbcreator**aus, und klicken Sie dann auf **OK**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image14.png)

Um nachfolgende bereit Stellungen zu unterstützen, müssen Sie nach der ersten Bereitstellung auch das Bereitstellungs Konto der **DB-\_** Rolle "Besitzer" in der Datenbank hinzufügen. Dies liegt daran, dass Sie bei nachfolgenden bereit Stellungen das Schema einer vorhandenen Datenbank ändern, anstatt eine neue Datenbank zu erstellen. Wie im vorherigen Abschnitt beschrieben, können Sie einen Benutzer nicht zu einer Daten Bank Rolle hinzufügen, bis Sie die Datenbank aus offensichtlichen Gründen erstellt haben.

**So ordnen Sie die Anmelde Informationen für das Bereitstellungs Konto der Daten Bank Rolle "DB\_Owner**

1. Öffnen Sie SQL Server Management Studio wie zuvor.
2. Erweitern Sie im Fenster **Objekt-Explorer** den Knoten **Sicherheit** , erweitern Sie den Knoten **Anmeldungen** , und doppelklicken Sie dann auf die Anmelde Informationen des Computer Kontos (z. b. **fabrikam\matt**).
3. Klicken Sie im Dialogfeld **Anmeldungs Eigenschaften** auf **Benutzer Zuordnung**.
4. Wählen Sie in der Liste Benutzer, die **dieser Anmeldung zugeordnet** sind den Namen der Datenbank aus (z. **b. ContactManager**).
5. Wählen Sie in der Liste **Mitgliedschaft in Daten Bank Rolle für:** *[Datenbankname]* die Rolle **DB-\_Besitzer** aus.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image15.png)
6. Klicken Sie auf **OK**.

## <a name="conclusion"></a>Zusammenfassung

Der Datenbankserver sollte jetzt bereit sein, Remote-Daten Bank Bereitstellungen zu akzeptieren und Remote-IIS-Webserver den Zugriff auf Ihre Datenbanken zu ermöglichen. Bevor Sie versuchen, Datenbanken bereitzustellen und zu verwenden, sollten Sie diese wichtigen Punkte überprüfen:

- Haben Sie SQL Server für die Annahme von TCP/IP-Remote Verbindungen konfiguriert?
- Haben Sie Firewalls konfiguriert, um SQL Server Datenverkehr zuzulassen?
- Haben Sie für jeden Webserver, der auf SQL Server zugreifen soll, eine Computer Kontoanmeldung erstellt?
- Umfasst die Daten Bank Bereitstellung ein Skript zum Erstellen von Benutzer Rollen Zuordnungen, oder müssen Sie diese manuell erstellen, nachdem Sie die Datenbank zum ersten Mal bereitgestellt haben?
- Haben Sie einen Anmelde Namen für das Bereitstellungs Konto erstellt und der **dbcreator** -Server Rolle hinzugefügt?

## <a name="further-reading"></a>Weitere nützliche Informationen

Anleitungen zum Bereitstellen von Datenbankprojekten finden Sie unter Bereitstellen von [Datenbankprojekten](../web-deployment-in-the-enterprise/deploying-database-projects.md). Anleitungen zum Erstellen von Daten bankrollen Mitgliedschaften durch Ausführen eines Skripts nach der Bereitstellung finden Sie unter Bereitstellen von [Daten bankrollen Mitgliedschaften in Test Umgebungen](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). Anleitungen zum erfüllen der besonderen Herausforderungen bei der Bereitstellung, die für Mitgliedschafts Datenbanken stehen, finden Sie unter Bereitstellen von [Mitgliedschafts Datenbanken für Unternehmensumgebungen](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

> [!div class="step-by-step"]
> [Zurück](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
> [Weiter](creating-a-server-farm-with-the-web-farm-framework.md)
