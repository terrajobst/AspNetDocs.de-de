---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment
title: Konfigurieren eines Webservers für die Web deploy Veröffentlichung (Offline Bereitstellung) | Microsoft-Dokumentation
author: jrjlee
description: In diesem Thema wird beschrieben, wie Sie einen IIS-Webserver für die Unterstützung der Offline Veröffentlichung und-Bereitstellung konfigurieren. Wenn Sie mit Internetinformationsdienste arbeiten (I...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: ba92788f-9f03-44b1-b6b2-af8413e6a35d
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment
msc.type: authoredcontent
ms.openlocfilehash: f93cf11085fb19afb97b71aca8f638bd88fe658b
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74621100"
---
# <a name="configuring-a-web-server-for-web-deploy-publishing-offline-deployment"></a>Konfigurieren eines Webservers für die Web Deploy-Veröffentlichung (Offlinebereitstellung)

von [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird beschrieben, wie Sie einen IIS-Webserver für die Unterstützung der Offline Veröffentlichung und-Bereitstellung konfigurieren.
> 
> Wenn Sie mit Internetinformationsdienste (IIS)-Webbereitstellungs Tool (Web deploy) 2,0 oder höher arbeiten, gibt es drei Hauptansätze, die Sie verwenden können, um Ihre Anwendungen oder Websites auf einen Webserver zu übernehmen. Sie haben folgende Möglichkeiten:
> 
> - Verwenden Sie den *Web deploy Remote-Agent-Dienst*. Diese Vorgehensweise erfordert eine geringere Konfiguration des Webservers, aber Sie müssen die Anmelde Informationen eines lokalen Server Administrators bereitstellen, um alles auf dem Server bereitzustellen.
> - Verwenden Sie den *Web deploy Handler*. Diese Vorgehensweise ist weitaus komplexer und erfordert mehr Aufwand für die Einrichtung des Webservers. Wenn Sie diesen Ansatz verwenden, können Sie jedoch IIS so konfigurieren, dass Benutzer, die nicht Administratoren sind, die Bereitstellung durchführen können. Der Web deploy Handler ist nur in IIS, Version 7 oder höher, verfügbar.
> - *Offline Bereitstellung*verwenden. Diese Vorgehensweise erfordert die geringste Konfiguration des Webservers, aber ein Server Administrator muss das Webpaket manuell auf den Server kopieren und über den IIS-Manager importieren.
> 
> Weitere Informationen zu den wichtigsten Features, Vorteilen und Nachteile dieser Ansätze finden [Sie unter Auswählen des richtigen Ansatzes für die Webbereitstellung](choosing-the-right-approach-to-web-deployment.md).

Ja, wenn die Netzwerkinfrastruktur oder Sicherheitseinschränkungen die Remote Bereitstellung verhindern. Dies ist höchstwahrscheinlich der Fall in Produktionsumgebungen mit Internet Zugriff, bei denen die Webserver entweder physisch oder durch&#x2014;Firewalls und Subnetze&#x2014;von der restlichen Serverinfrastruktur isoliert sind.

Diese Vorgehensweise wird natürlich weniger wünschenswert, wenn Ihre Webanwendungen regelmäßig aktualisiert werden. Wenn Ihre Infrastruktur dies zulässt, sollten Sie die Aktivierung der Remote Bereitstellung in Erwägung ziehen, indem Sie entweder den Web deploy Handler oder den Web deploy Remote-Agent-Dienst verwenden.

## <a name="task-overview"></a>Aufgaben Übersicht

Zum Konfigurieren des Webservers für die Unterstützung des Offline Imports und der Bereitstellung von Webpaketen müssen Sie folgende Schritte ausführen:

- Installieren Sie IIS 7,5 und die empfohlene IIS 7-Konfiguration.
- Installieren Sie Web deploy 2,1 oder höher.
- Erstellen Sie eine IIS-Website zum Hosten des bereitgestellten Inhalts.
- Deaktivieren Sie den Web Deployment Agent-Dienst.

Um die Beispiellösung speziell zu hosten, müssen Sie auch folgende Schritte ausführen:

- Installieren Sie den .NET Framework 4,0.
- Installieren Sie ASP.NET MVC 3.

In diesem Thema wird gezeigt, wie Sie die einzelnen Prozeduren ausführen. Bei den Aufgaben und exemplarischen Vorgehensweisen in diesem Thema wird davon ausgegangen, dass Sie mit einem Clean Server Build beginnen, auf dem Windows Server 2008 R2 ausgeführt wird. Bevor Sie fortfahren, stellen Sie Folgendes sicher:

- Windows Server 2008 R2 Service Pack 1 und alle verfügbaren Updates werden installiert.
- Der Server ist einer Domäne beigetreten.
- Der Server verfügt über eine statische IP-Adresse.

> [!NOTE]
> Weitere Informationen zum Hinzufügen von Computern zu einer Domäne finden Sie unter [Hinzufügen von Computern zur Domäne und anmelden](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Weitere Informationen zum Konfigurieren statischer IP-Adressen finden Sie unter [Konfigurieren einer statischen IP-Adresse](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx).

## <a name="install-products-and-components"></a>Installieren von Produkten und Komponenten

In diesem Abschnitt werden Sie durch die Installation der erforderlichen Produkte und Komponenten auf dem Webserver geführt. Bevor Sie beginnen, empfiehlt es sich, Windows Update auszuführen, um sicherzustellen, dass der Server vollständig auf dem neuesten Stand ist.

In diesem Fall müssen Sie Folgendes installieren:

- **Empfohlene Konfiguration für IIS 7**. Dadurch wird die Rolle " **Webserver (IIS)** " auf dem Webserver aktiviert und der Satz von IIS-Modulen und-Komponenten installiert, die Sie benötigen, um eine ASP.NET-Anwendung zu hosten.
- **.NET Framework 4,0**. Dies ist erforderlich, um Anwendungen auszuführen, die auf dieser Version des .NET Framework erstellt wurden.
- **Webbereitstellungs Tool 2,1 oder**höher. Dadurch wird Web deploy (und die zugrunde liegende ausführbare Datei msbereitstellung. exe) auf dem Server installiert. Web deploy in IIS integriert und ermöglicht es Ihnen, Webpakete zu importieren und zu exportieren.
- **ASP.NET MVC 3**. Dadurch werden die Assemblys installiert, die Sie zum Ausführen von MVC 3-Anwendungen benötigen

> [!NOTE]
> In dieser exemplarischen Vorgehensweise wird die Verwendung des Webplattform-Installers zum Installieren und Konfigurieren verschiedener Komponenten beschrieben. Obwohl Sie den Webplattform-Installer nicht verwenden müssen, wird der Installationsvorgang vereinfacht, da Abhängigkeiten automatisch erkannt werden und sichergestellt wird, dass Sie immer die neuesten Produktversionen erhalten. Weitere Informationen finden Sie unter [Microsoft-Webplattform-Installer 3,0](https://go.microsoft.com/?linkid=9805118).

**So installieren Sie die erforderlichen Produkte und Komponenten**

1. Herunterladen und Installieren des [Webplattform-Installers](https://go.microsoft.com/?linkid=9805118).
2. Nach Abschluss der Installation wird der Webplattform-Installer automatisch gestartet.

    > [!NOTE]
    > Sie können den Webplattform-Installer jetzt jederzeit über das **Startmenü** starten. Klicken Sie hierzu im **Startmenü** auf **Alle Programme**, und klicken Sie dann auf **Microsoft-Webplattform-Installer**.
3. Klicken Sie oben im Fenster **Webplattform-Installer 3,0** auf **Produkte**.
4. Klicken Sie im Navigationsbereich auf der linken Seite des Fensters auf **Frameworks**.
5. Wenn die .NET Framework in der Zeile **Microsoft .NET Framework 4** nicht bereits installiert ist, klicken Sie auf **Hinzufügen**.

    > [!NOTE]
    > Möglicherweise haben Sie den .NET Framework 4,0 bereits über Windows Update installiert. Wenn ein Produkt oder eine Komponente bereits installiert ist, zeigt der Webplattform-Installer dies an, indem die Schaltfläche " **Hinzufügen** " durch den **installierten**Text ersetzt wird.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image1.png)
6. Klicken Sie in der Zeile **ASP.NET MVC 3 (Visual Studio 2010)** auf **Hinzufügen**.
7. Klicken Sie im Navigationsbereich auf **Server**.
8. Klicken Sie in der Zeile mit der **empfohlenen IIS 7-Konfiguration** auf **Hinzufügen**.
9. Klicken Sie in der Zeile **Webbereitstellungs Tool 2,1** auf **Hinzufügen**.
10. Klicken Sie auf **Installieren**. Der Webplattform-Installer zeigt eine Liste der Produkte&#x2014;zusammen mit allen zugehörigen Abhängigkeiten&#x2014;an, die installiert werden müssen, und fordert Sie auf, die Lizenzbedingungen zu akzeptieren.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image2.png)
11. Lesen Sie die Lizenzbedingungen, und klicken Sie auf **akzeptieren**, wenn Sie den Bedingungen zustimmen.
12. Wenn die Installation abgeschlossen ist, klicken Sie auf **Fertig**stellen, und schließen Sie dann das Fenster **Webplattform-Installer 3,0** .

Wenn Sie den .NET Framework 4,0 vor der Installation von IIS installiert haben, müssen Sie das [ASP.NET IIS-Registrierungs Tool](https://msdn.microsoft.com/library/k6h9cz8h(v=VS.100).aspx) (ASPNET\_regiis. exe) ausführen, um die neueste Version von ASP.net bei IIS zu registrieren. Wenn Sie dies nicht tun, werden Sie feststellen, dass IIS statische Inhalte (z. b. HTML-Dateien) ohne Probleme bereitstellt, aber der **HTTP-Fehler 404,0 – nicht gefunden** wird, wenn Sie versuchen, zu ASP.net-Inhalt zu navigieren. Sie können das nächste Verfahren verwenden, um sicherzustellen, dass ASP.NET 4,0 registriert ist.

**So registrieren Sie ASP.NET 4,0 mit IIS**

1. Klicken Sie auf **Start**, und geben Sie dann **Command Prompt**ein.
2. Klicken Sie in den Suchergebnissen mit der rechten Maustaste auf **Eingabeaufforderung**, und klicken Sie dann auf **als Administrator ausführen**.
3. Navigieren Sie im Eingabe Aufforderungs Fenster zum Verzeichnis **%windir%\Microsoft.Net\Framework\v4.0.30319** .
4. Geben Sie diesen Befehl ein, und drücken Sie dann die EINGABETASTE:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/samples/sample1.cmd)]
5. Wenn Sie beabsichtigen, 64-Bit-Webanwendungen zu einem beliebigen Zeitpunkt zu hosten, sollten Sie auch die 64-Bit-Version von ASP.net mit IIS registrieren. Navigieren Sie hierzu im Eingabe Aufforderungs Fenster zum Verzeichnis **%windir%\Microsoft.NET\Framework64\v4.0.30319** .
6. Geben Sie diesen Befehl ein, und drücken Sie dann die EINGABETASTE:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/samples/sample2.cmd)]

Verwenden Sie als bewährte Vorgehensweise Windows Update an dieser Stelle erneut, um alle verfügbaren Updates für die neuen Produkte und Komponenten herunterzuladen und zu installieren, die Sie installiert haben.

## <a name="configure-the-iis-website"></a>IIS-Website konfigurieren

Bevor Sie Webinhalte auf Ihrem Server bereitstellen können, müssen Sie eine IIS-Website zum Hosten der Inhalte erstellen und konfigurieren. Web deploy können nur Webpakete für eine vorhandene IIS-Website bereitstellen. die Website kann nicht für Sie erstellt werden. Auf hoher Ebene müssen Sie diese Aufgaben ausführen:

- Erstellen Sie einen Ordner im Dateisystem, in dem Ihre Inhalte gehostet werden.
- Erstellen Sie eine IIS-Website, um die Inhalte bereitzustellen, und ordnen Sie Sie dem lokalen Ordner zu.
- Erteilen Sie der Anwendungs Pool Identität für den lokalen Ordner Leseberechtigungen.

Obwohl es nichts hindert, Inhalte auf der Standard Website in IIS bereitzustellen, wird dieser Ansatz für andere als Test-oder Demonstrations Szenarien nicht empfohlen. Zum Simulieren einer Produktionsumgebung sollten Sie eine neue IIS-Website mit Einstellungen erstellen, die für die Anforderungen Ihrer Anwendung spezifisch sind.

**So erstellen und konfigurieren Sie eine IIS-Website**

1. Erstellen Sie auf dem lokalen Dateisystem einen Ordner zum Speichern der Inhalte (z. b. **c:\demosite**).
2. Zeigen Sie im **Startmenü** auf **Verwaltung**, und klicken Sie dann auf **Internetinformationsdienste (IIS)-Manager**.
3. Erweitern Sie im IIS-Manager im Bereich **Verbindungen** den Server Knoten (z. b. **PROWEB1**).

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image3.png)
4. Klicken Sie mit der rechten Maustaste auf den Knoten **Sites** , und klicken Sie dann auf **Website hinzufügen**.
5. Geben Sie im Feld **Website Name** einen Namen für die IIS-Website ein (z. b. **Demosite**).
6. Geben Sie im Feld **physischer Pfad** den Pfad zum lokalen Ordner ein (oder suchen Sie danach) (z. b. **c:\demosite**).
7. Geben Sie im Feld **Port** die Portnummer ein, auf der die Website gehostet werden soll (z. b. **85**).

    > [!NOTE]
    > Die Standard Portnummern lauten 80 für http und 443 für HTTPS. Wenn Sie diese Website jedoch auf Port 80 hosten, müssen Sie die Standard Website abbrechen, bevor Sie auf Ihre Website zugreifen können.
8. Lassen Sie das Feld **Hostname** leer, außer wenn Sie einen Domain Name System Eintrag (DNS) für die Website konfigurieren möchten, und klicken Sie dann auf **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image4.png)

    > [!NOTE]
    > In einer Produktionsumgebung möchten Sie wahrscheinlich Ihre Website auf Port 80 hosten und einen Host Header sowie übereinstimmende DNS-Einträge konfigurieren. Weitere Informationen zum Konfigurieren von Host Headern in IIS 7 finden Sie unter [Konfigurieren eines Host Headers für eine Website (IIS 7)](https://technet.microsoft.com/library/cc753195(WS.10).aspx). Weitere Informationen zur DNS-Server Rolle in Windows Server 2008 R2 finden Sie unter [Übersicht über DNS-Server](https://technet.microsoft.com/library/cc770392.aspx) und [DNS-Server](https://technet.microsoft.com/windowsserver/dd448607).
9. Klicken Sie im **Aktions** Bereich unter **Website bearbeiten**auf **Bindungen**.
10. Klicken Sie im Dialogfeld **Site Bindungen** auf **Hinzufügen**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image5.png)
11. Legen Sie im Dialogfeld **Site Bindung hinzufügen** die **IP-Adresse** und den **Port** entsprechend der vorhandenen Standort Konfiguration fest.
12. Geben Sie im Feld **Hostname** den Namen des Webservers ein (z. b. **PROWEB1**), und klicken Sie dann auf **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image6.png)

    > [!NOTE]
    > Mit der ersten Site Bindung können Sie mithilfe der IP-Adresse und des Ports oder `http://localhost:85`lokal auf die Website zugreifen. Die zweite Site Bindung ermöglicht den Zugriff auf die Website von anderen Computern in der Domäne mithilfe des Computer namens (z. b. http://proweb1:85).
13. Klicken Sie im Dialogfeld **Site Bindungen** auf **Schließen**.
14. Klicken Sie im Bereich **Verbindungen** auf **Anwendungs Pools**.
15. Klicken Sie im Bereich **Anwendungs Pools** mit der rechten Maustaste auf den Namen des Anwendungs Pools, und klicken Sie dann auf **Grundeinstellungen**. Standardmäßig entspricht der Name Ihres Anwendungs Pools dem Namen Ihrer Website (z. b. **Demosite**).
16. Wählen Sie in der Liste **.NET Framework Version** **.NET Framework v 4.0.30319**aus, und klicken Sie dann auf **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image7.png)

    > [!NOTE]
    > Die Beispiellösung erfordert .NET Framework 4,0. Dies ist keine Voraussetzung für Web deploy im Allgemeinen.

Damit Ihre Website Inhalte bereitstellen kann, muss die Anwendungs Pool Identität über Leseberechtigungen für den lokalen Ordner verfügen, in dem der Inhalt gespeichert wird. In IIS 7,5 werden Anwendungs Pools standardmäßig mit einer eindeutigen Anwendungs Pool Identität ausgeführt (im Gegensatz zu früheren Versionen von IIS, bei denen Anwendungs Pools in der Regel mit dem Netzwerkdienst Konto ausgeführt werden). Die Anwendungs Pool Identität ist kein echtes Benutzerkonto und wird nicht in einer Liste von Benutzern oder Gruppen&#x2014;angezeigt. Sie wird automatisch erstellt, wenn der Anwendungs Pool gestartet wird. Jede Anwendungs Pool Identität wird der lokalen IIS- **\_IUSRS** -Sicherheitsgruppe als ausgeblendetes Element hinzugefügt.

Zum Erteilen von Berechtigungen für eine Anwendungs Pool Identität in einer Datei oder einem Ordner haben Sie zwei Möglichkeiten:

- Weisen Sie der Anwendungs Pool Identität direkt Berechtigungen zu, und verwenden Sie dabei das Format <strong>IIS AppPool\</strong ><em>[Anwendungs Pool Name]</em>(z. b. <strong>IIS apppool\demosite</strong>).
- Weisen Sie der **IIS-\_ius RS** -Gruppe Berechtigungen zu.

Der gängigste Ansatz besteht darin, der lokalen IIS- **\_ihirs** -Gruppe Berechtigungen zuzuweisen, da Sie mit diesem Ansatz Anwendungs Pools ändern können, ohne die Dateisystem Berechtigungen neu zu konfigurieren. Im nächsten Verfahren wird dieser Gruppenbasierte Ansatz verwendet.

> [!NOTE]
> Weitere Informationen zu Anwendungs Pool Identitäten in IIS 7,5 finden Sie unter [Anwendungs Pool Identitäten](https://go.microsoft.com/?linkid=9805123).

**So konfigurieren Sie Ordner Berechtigungen für eine IIS-Website**

1. Navigieren Sie in Windows-Explorer zum Speicherort des lokalen Ordners.
2. Klicken Sie mit der rechten Maustaste auf den Ordner, und klicken Sie auf **Eigenschaften**.
3. Klicken Sie auf der Registerkarte **Sicherheit** auf **Bearbeiten**, und klicken Sie dann auf **Hinzufügen**.
4. Klicken Sie auf **Standorte**. Wählen Sie im Dialogfeld Speicher **Orte** den lokalen Server aus, und klicken Sie dann auf **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image8.png)
5. Geben Sie im Dialogfeld **Benutzer oder Gruppen auswählen** **IIS\_ius RS**ein, klicken Sie auf **Namen überprüfen**, und klicken Sie dann auf **OK**.
6. Beachten Sie im Dialogfeld <strong>Berechtigungen für</strong><em>[Ordnername]</em> , dass der neuen Gruppe standardmäßig die Berechtigungen <strong>Lesen &amp; ausführen</strong>, <strong>Ordner Inhalt auflisten</strong>und <strong>Lesen</strong> zugewiesen wurden. Bleiben Sie unverändert, und klicken Sie auf <strong>OK</strong>.
7. Klicken Sie auf <strong>OK</strong> , um das Dialogfeld <em>[Ordnername]</em><strong>Eigenschaften</strong> zu schließen.

## <a name="disable-the-remote-agent-service"></a>Remote-Agent-Dienst deaktivieren

Wenn Sie Web deploy installieren, wird der Web Deployment Agent-Dienst automatisch installiert und gestartet. Dieser Dienst ermöglicht Ihnen das Bereitstellen und Veröffentlichen von Webpaketen an einem Remote Speicherort. In diesem Szenario verwenden Sie die Remote Bereitstellungs Funktion nicht, daher sollten Sie den Dienst abbrechen und deaktivieren.

> [!NOTE]
> Der Remote-Agent-Dienst muss nicht angehalten werden, um ein Webpaket manuell zu importieren und bereitzustellen. Es empfiehlt sich jedoch, den Dienst anzuhalten und zu deaktivieren, wenn Sie ihn nicht verwenden möchten.

Sie können einen Dienst auf verschiedene Weise mit verschiedenen Befehlszeilen Programmen oder Windows PowerShell-Cmdlets abbrechen und deaktivieren. In diesem Verfahren wird ein einfacher Benutzeroberflächen basierter Ansatz beschrieben.

**So wird der Remote-Agent-Dienst beendet und deaktiviert**

1. Zeigen Sie im **Startmenü** auf **Verwaltung**, und klicken Sie dann auf **Dienste**.
2. Suchen Sie in der Dienste-Konsole die Zeile **Web Deployment Agent-Dienst** .

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image9.png)
3. Klicken Sie mit der rechten Maustaste auf **Web Deployment Agent Service**, und klicken Sie dann auf **Eigenschaften**.
4. Klicken Sie im Dialogfeld **Eigenschaften des webDeployment Agent Diensts** auf " **Abbrechen**".
5. Wählen Sie in der Liste **Starttyp** die Option **deaktiviert**aus, und klicken Sie dann auf **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image10.png)

## <a name="conclusion"></a>Schlussfolgerung

An diesem Punkt ist der Webserver für die Offline Bereitstellung des Webpakets bereit. Bevor Sie versuchen, Webpakete in eine IIS-Website zu importieren, sollten Sie diese wichtigen Punkte überprüfen:

- Haben Sie ASP.NET 4,0 bei IIS registriert?
- Hat die Anwendungs Pool Identität Lesezugriff auf den Quellordner für Ihre Website?
- Haben Sie den Web Deployment Agent-Dienst angehalten?

> [!div class="step-by-step"]
> [Zurück](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)
> [Weiter](configuring-a-database-server-for-web-deploy-publishing.md)
