---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler
title: Konfigurieren eines Webservers für die Web deploy Veröffentlichung (Web deploy Handler) | Microsoft-Dokumentation
author: jrjlee
description: In diesem Thema wird beschrieben, wie ein Internetinformationsdienste (IIS)-Webserver für die Unterstützung der Webveröffentlichung und-Bereitstellung mit IIS Web deploy Han konfiguriert wird.
ms.author: riande
ms.date: 01/29/2017
ms.assetid: 90ebf911-1c46-4470-b876-1335bd0f590f
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler
msc.type: authoredcontent
ms.openlocfilehash: baaebd32f08d3c6b861572c5c5a16ec0fb70aaf0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78458637"
---
# <a name="configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler"></a>Konfigurieren eines Webservers für die Web Deploy-Veröffentlichung (Web Deploy-Handler)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird beschrieben, wie ein Internetinformationsdienste (IIS)-Webserver für die Unterstützung der Webveröffentlichung und-Bereitstellung mit dem IIS-Web deploy Handler konfiguriert wird.
> 
> Wenn Sie mit Web deploy 2,0 oder höher arbeiten, gibt es drei Hauptansätze, die Sie verwenden können, um Ihre Anwendungen oder Websites auf einen Webserver zu übernehmen. Sie haben folgende Möglichkeiten:
> 
> - Verwenden Sie den *Web deploy Remote-Agent-Dienst*. Diese Vorgehensweise erfordert eine geringere Konfiguration des Webservers, aber Sie müssen die Anmelde Informationen eines lokalen Server Administrators bereitstellen, um alles auf dem Server bereitzustellen.
> - Verwenden Sie den *Web deploy Handler*. Diese Vorgehensweise ist weitaus komplexer und erfordert mehr Aufwand für die Einrichtung des Webservers. Wenn Sie diesen Ansatz verwenden, können Sie jedoch IIS so konfigurieren, dass Benutzer, die nicht Administratoren sind, die Bereitstellung durchführen können. Der Web deploy Handler ist nur in IIS, Version 7 oder höher, verfügbar.
> - *Offline Bereitstellung*verwenden. Diese Vorgehensweise erfordert die geringste Konfiguration des Webservers, aber ein Server Administrator muss das Webpaket manuell auf den Server kopieren und über den IIS-Manager importieren.
> 
> Weitere Informationen zu den wichtigsten Features, Vorteilen und Nachteile dieser Ansätze finden [Sie unter Auswählen des richtigen Ansatzes für die Webbereitstellung](choosing-the-right-approach-to-web-deployment.md).

Ja, wenn Sie Benutzern, die nicht Administratoren sind, das Bereitstellen von Inhalten für bestimmte IIS-Websites erlauben möchten. Diese Vorgehensweise ist häufig in folgenden Szenarios wünschenswert:

- Staging-oder Produktionsumgebungen, in denen die Person oder das Dienst Konto, das die Remote Bereitstellung auslöst, wahrscheinlich keinen Zugriff auf die Anmelde Informationen eines Server Administrators hat.
- Gehostete Umgebungen, in denen Sie Remote Benutzern die Möglichkeit geben möchten, ihre Websites zu aktualisieren, ohne Ihnen vollständige Kontrolle über Ihre Webserver (oder den Zugriff auf die Websites von anderen Personen) zu gewähren.

In Entwicklungs-und Testszenarien oder in kleineren Organisationen ist das Bereitstellen von Inhalten mithilfe von Anmelde Informationen des Server Administrators häufig weniger umstritten. In diesen Szenarien bietet das Konfigurieren von Webservern für die Unterstützung der Bereitstellung mithilfe des [Web deploy Remote-Agent-Diensts](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md) einen einfacheren Ansatz.

## <a name="task-overview"></a>Aufgaben Übersicht

Zum Konfigurieren des Webservers für das akzeptieren und Bereitstellen von Webpaketen von einem Remote Computer mithilfe des Web deploy handleransatzes müssen Sie folgende Schritte ausführen:

- Erstellen Sie ein Domänen Benutzerkonto ("nicht Administrator Benutzer"), oder wählen Sie ein Domänen Benutzerkonto aus, dessen Anmelde Informationen Sie zum Durchführen von bereit Stellungen verwenden.
- Installieren Sie IIS 7,5, einschließlich des Webverwaltungsdiensts und des Basis Authentifizierungs Moduls.
- Installieren Sie Web deploy 2,1 oder höher.
- Konfigurieren Sie den Webverwaltungsdienst so, dass Remote Verbindungen zugelassen werden, und starten Sie den Dienst.
- Erstellen Sie eine IIS-Website zum Hosten des bereitgestellten Inhalts.
- Erteilen Sie Ihren nicht-Administrator-Benutzerberechtigungen für Ihre Website im IIS-Manager.
- Stellen Sie sicher, dass die Delegierungs Regeln des Webverwaltungsdiensts es dem Dienst ermöglichen, Website Inhalte mithilfe Ihres nicht Administrator-Benutzerkontos hinzuzufügen und zu ändern.
- Konfigurieren Sie alle Firewalls, um eingehende Verbindungen an Port 8172 zuzulassen.

Um die ContactManager-Beispiellösung speziell zu hosten, müssen Sie auch Folgendes ausführen:

- Installieren Sie den .NET Framework 4,0.
- Installieren Sie ASP.NET MVC 3.

In diesem Thema wird gezeigt, wie Sie die einzelnen Prozeduren ausführen. Bei den Aufgaben und exemplarischen Vorgehensweisen in diesem Thema wird davon ausgegangen, dass Sie mit einem Clean Server Build beginnen, auf dem Windows Server 2016 ausgeführt wird. Bevor Sie fortfahren, stellen Sie Folgendes sicher:

- Windows Server 2016
- Der Server ist einer Domäne beigetreten.
- Der Server verfügt über eine statische IP-Adresse.

> [!NOTE]
> Weitere Informationen zum Hinzufügen von Computern zu einer Domäne finden Sie unter [Hinzufügen von Computern zur Domäne und anmelden](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Weitere Informationen zum Konfigurieren statischer IP-Adressen finden Sie unter [Konfigurieren einer statischen IP-Adresse](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx).

## <a name="install-products-and-components"></a>Installieren von Produkten und Komponenten

In diesem Abschnitt werden Sie durch die Installation der erforderlichen Produkte und Komponenten auf dem Webserver geführt. Bevor Sie beginnen, empfiehlt es sich, Windows Update auszuführen, um sicherzustellen, dass der Server vollständig auf dem neuesten Stand ist.

In diesem Fall müssen Sie Folgendes installieren:

- **Empfohlene Konfiguration für IIS 7**. Dadurch wird die Rolle " **Webserver (IIS)** " auf dem Webserver aktiviert und der Satz von IIS-Modulen und-Komponenten installiert, die Sie benötigen, um eine ASP.NET-Anwendung zu hosten.
- **IIS: Verwaltungsdienst**. Dadurch wird der Webverwaltungsdienst (WmSvc) in IIS installiert. Dieser Dienst ermöglicht die Remote Verwaltung von IIS-Websites und macht den Endpunkt des Web deploy Handlers für Clients verfügbar.
- **IIS: Standard Authentifizierung**. Dadurch wird das IIS-Standard Authentifizierungs Modul installiert. Dadurch kann der Webverwaltungsdienst (WmSvc) die von Ihnen bereitgestellten Anmelde Informationen authentifizieren.
- **Webbereitstellungs Tool 2,1 oder**höher. Dadurch wird Web deploy (und die zugrunde liegende ausführbare Datei msbereitstellung. exe) auf dem Server installiert. Im Rahmen dieses Prozesses wird der Web deploy Handler installiert und in den Webverwaltungsdienst integriert.
- **.NET Framework 4,0**. Dies ist erforderlich, um Anwendungen auszuführen, die auf dieser Version des .NET Framework erstellt wurden.
- **ASP.NET MVC 3**. Dadurch werden die Assemblys installiert, die Sie zum Ausführen von MVC 3-Anwendungen benötigen

> [!NOTE]
> In dieser exemplarischen Vorgehensweise wird die Verwendung des Webplattform-Installers zum Installieren und Konfigurieren verschiedener Komponenten beschrieben. Obwohl Sie den Webplattform-Installer nicht verwenden müssen, wird der Installationsvorgang vereinfacht, da Abhängigkeiten automatisch erkannt werden und sichergestellt wird, dass Sie immer die neuesten Produktversionen erhalten. Weitere Informationen finden Sie unter [Microsoft-Webplattform-Installer](https://go.microsoft.com/?linkid=9805118).

**So installieren Sie die erforderlichen Produkte und Komponenten**

1. Herunterladen und Installieren des [Webplattform-Installers](https://go.microsoft.com/?linkid=9805118).
2. Nach Abschluss der Installation wird der Webplattform-Installer automatisch gestartet.

    > [!NOTE]
    > Sie können den Webplattform-Installer jetzt jederzeit über das **Startmenü** starten. Klicken Sie hierzu im **Startmenü** auf **Alle Programme**, und klicken Sie dann auf **Microsoft-Webplattform-Installer**.
3. Klicken Sie oben im Fenster **Webplattform-Installer** auf **Produkte**.
4. Klicken Sie im Navigationsbereich auf der linken Seite des Fensters auf **Frameworks**.
5. Wenn die .NET Framework in der Zeile **Microsoft .NET Framework 4** nicht bereits installiert ist, klicken Sie auf **Hinzufügen**.

    > [!NOTE]
    > Möglicherweise haben Sie den .NET Framework 4,0 bereits über Windows Update installiert. Wenn ein Produkt oder eine Komponente bereits installiert ist, zeigt der Webplattform-Installer dies an, indem die Schaltfläche " **Hinzufügen** " durch den **installierten**Text ersetzt wird.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image1.png)
6. Klicken Sie in der Zeile **ASP.NET MVC 3 (Visual Studio 2010)** auf **Hinzufügen**.
7. Klicken Sie im Navigationsbereich auf **Server**.
8. Klicken Sie in der Zeile mit der **empfohlenen IIS 7-Konfiguration** auf **Hinzufügen**.
9. Klicken Sie in der Zeile **Webbereitstellungs Tool 2,1** auf **Hinzufügen**.
10. Klicken Sie in der Zeile **IIS: Standard Authentifizierung** auf **Hinzufügen**.
11. Klicken Sie in der Zeile **IIS: Verwaltungsdienst** auf **Hinzufügen**.
12. Klicken Sie auf **Installieren**. Der Webplattform-Installer zeigt eine Liste der Produkte&#x2014;zusammen mit allen zugehörigen Abhängigkeiten&#x2014;an, die installiert werden müssen, und fordert Sie auf, die Lizenzbedingungen zu akzeptieren.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image2.png)
13. Lesen Sie die Lizenzbedingungen, und klicken Sie auf **akzeptieren**, wenn Sie den Bedingungen zustimmen.
14. Wenn die Installation abgeschlossen ist, klicken Sie auf **Fertig**stellen, und schließen Sie dann das Fenster **Webplattform-Installer** .

Wenn Sie den .NET Framework 4,0 vor der Installation von IIS installiert haben, müssen Sie das [ASP.NET IIS-Registrierungs Tool](https://msdn.microsoft.com/library/k6h9cz8h(v=VS.100).aspx) (ASPNET\_regiis. exe) ausführen, um die neueste Version von ASP.net bei IIS zu registrieren. Wenn Sie dies nicht tun, werden Sie feststellen, dass IIS statische Inhalte (z. b. HTML-Dateien) ohne Probleme bereitstellt, aber der **HTTP-Fehler 404,0 – nicht gefunden** wird, wenn Sie versuchen, zu ASP.net-Inhalt zu navigieren. Sie können das nächste Verfahren verwenden, um sicherzustellen, dass ASP.NET 4,0 registriert ist.

**So registrieren Sie ASP.NET 4,0 mit IIS**

1. Klicken Sie auf **Start**, und geben Sie dann **Command Prompt**ein.
2. Klicken Sie in den Suchergebnissen mit der rechten Maustaste auf **Eingabeaufforderung**, und klicken Sie dann auf **als Administrator ausführen**.
3. Navigieren Sie im Eingabe Aufforderungs Fenster zum Verzeichnis **%windir%\Microsoft.Net\Framework\v4.0.30319** .
4. Geben Sie diesen Befehl ein, und drücken Sie dann die EINGABETASTE:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/samples/sample1.cmd)]
5. Wenn Sie beabsichtigen, 64-Bit-Webanwendungen zu einem beliebigen Zeitpunkt zu hosten, sollten Sie auch die 64-Bit-Version von ASP.net mit IIS registrieren. Navigieren Sie hierzu im Eingabe Aufforderungs Fenster zum Verzeichnis **%windir%\Microsoft.NET\Framework64\v4.0.30319** .
6. Geben Sie diesen Befehl ein, und drücken Sie dann die EINGABETASTE:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/samples/sample2.cmd)]

Verwenden Sie als bewährte Vorgehensweise Windows Update an dieser Stelle erneut, um alle verfügbaren Updates für die neuen Produkte und Komponenten herunterzuladen und zu installieren, die Sie installiert haben.

## <a name="configure-the-web-management-service"></a>Konfigurieren des Webverwaltungsdiensts

Nachdem Sie nun alles installiert haben, was Sie benötigen, ist der nächste Schritt die Konfiguration des Webverwaltungsdiensts in IIS. Auf hoher Ebene müssen Sie diese Aufgaben ausführen:

- Aktivieren Sie die Standard Authentifizierung auf Serverebene.
- Konfigurieren Sie den Webverwaltungsdienst so, dass Remote Verbindungen akzeptiert werden.
- Starten Sie den Webverwaltungsdienst.
- Überprüfen Sie, ob die erforderlichen Delegierungs Regeln für den Webverwaltungsdienst vorhanden sind.

**So konfigurieren Sie den Webverwaltungsdienst**

1. Zeigen Sie im **Startmenü** auf **Verwaltung**, und klicken Sie dann auf **Internetinformationsdienste (IIS)-Manager**.
2. Klicken Sie im IIS-Manager im Bereich **Verbindungen** auf den Server Knoten (z. b. **STAGEWEB1**).

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image3.png)
3. Doppelklicken Sie im mittleren Bereich unter **IIS**auf **Authentifizierung**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image20.png)
4. Klicken Sie mit der rechten Maustaste auf Standard **Authentifizierung**, und klicken Sie auf **aktivieren**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image5.png)
5. Klicken Sie im Bereich **Verbindungen** erneut auf den Server Knoten, um zu den Einstellungen der obersten Ebene zurückzukehren.
6. Doppelklicken Sie im mittleren Bereich unter **Verwaltung**auf **Verwaltungsdienst**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image6.png)
7. Wählen Sie im mittleren Bereich die Option **Remote Verbindungen aktivieren**aus.

    > [!NOTE]
    > Wenn der Webverwaltungsdienst bereits ausgeführt wird, müssen Sie ihn zuerst abbrechen.
8. Klicken Sie im Bereich **Aktionen** auf **starten** , um den Webverwaltungsdienst zu starten.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image7.png)
9. Wenn Sie aufgefordert werden, die Einstellungen zu speichern, klicken Sie auf **Ja**.

    > [!NOTE]
    > Sie können den Dienst auch so konfigurieren, dass er automatisch gestartet wird. Öffnen Sie hierzu die Konsole "Dienste", klicken Sie mit der rechten Maustaste auf **Webverwaltungsdienst**, und klicken Sie dann auf **Eigenschaften**. Wählen Sie in der Dropdown Liste **Starttyp** die Option **automatisch**aus, und klicken Sie dann auf **OK**.
10. Klicken Sie im Bereich **Verbindungen** erneut auf den Server Knoten, um zu den Einstellungen der obersten Ebene zurückzukehren.
11. Doppelklicken Sie im mittleren Bereich unter **Verwaltung**auf **Verwaltungsdienst Delegierung**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image8.png)
12. Vergewissern Sie sich, dass der Mittelbereich einen Regelsatz enthält.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image9.png)

    Diese Regeln ermöglichen autorisierten Webverwaltungsdienst-Benutzern die Verwendung verschiedener Web deploy Anbieter. Wenn Sie z. b. Webanwendungen und Inhalte in IIS über den Web deploy-Handler bereitstellen möchten, muss eine Delegierungs Regel vorhanden sein, mit der alle authentifizierten Webverwaltungsdienst-Benutzer die **contentPath** -und **iisapp** -Anbieter verwenden können (die letzte Regel, die Sie im Screenshot sehen können).

    Wenn Sie Produkte und Komponenten in der in diesem Thema beschriebenen Reihenfolge installiert haben, sollte der Webverwaltungsdienst in der aktuellen Version von Web deploy automatisch alle erforderlichen Delegierungs Regeln hinzugefügt werden. Wenn auf der Seite Delegierung des Verwaltungs Dienstanbieter keine Regeln angezeigt werden, müssen Sie diese selbst erstellen. Anweisungen hierzu finden Sie unter [Konfigurieren des Webbereitstellungs Handlers](https://go.microsoft.com/?linkid=9805124).
13. Klicken Sie im Bereich **Verbindungen** erneut auf den Server Knoten, um zu den Einstellungen der obersten Ebene zurückzukehren.

## <a name="create-and-configure-an-iis-website"></a>Erstellen und Konfigurieren einer IIS-Website

Bevor Sie Webinhalte auf Ihrem Server bereitstellen können, müssen Sie eine IIS-Website zum Hosten der Inhalte erstellen und konfigurieren. Web deploy können nur Webpakete für eine vorhandene IIS-Website bereitstellen. die Website kann nicht für Sie erstellt werden. Außerdem müssen Sie einige zusätzliche Konfigurationsschritte ausführen, damit ihr nicht-Administrator Konto Inhalte Remote bereitstellen kann. Auf hoher Ebene müssen Sie diese Aufgaben ausführen:

- Erstellen Sie einen Ordner im Dateisystem, in dem Ihre Inhalte gehostet werden.
- Erstellen Sie eine IIS-Website, um die Inhalte bereitzustellen, und ordnen Sie Sie dem lokalen Ordner zu.
- Erteilen Sie der Anwendungs Pool Identität für den lokalen Ordner Leseberechtigungen.
- Erteilen Sie dem Domänen Konto, mit dem Ihre Webanwendung bereitgestellt wird, die erforderlichen IIS-Berechtigungen.

Obwohl es nichts hindert, Inhalte auf der Standard Website in IIS bereitzustellen, wird dieser Ansatz für andere als Test-oder Demonstrations Szenarien nicht empfohlen. Zum Simulieren einer Produktionsumgebung sollten Sie eine neue IIS-Website mit Einstellungen erstellen, die für die Anforderungen Ihrer Anwendung spezifisch sind.

**So erstellen Sie eine IIS-Website**

1. Erstellen Sie auf dem lokalen Dateisystem einen Ordner zum Speichern der Inhalte (z. b. **c:\demosite**).
2. Zeigen Sie im **Startmenü** auf **Verwaltung**, und klicken Sie dann auf **Internetinformationsdienste (IIS)-Manager**.
3. Erweitern Sie im IIS-Manager im Bereich **Verbindungen** den Server Knoten (z. b. **STAGEWEB1**).

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image10.png)
4. Klicken Sie mit der rechten Maustaste auf den Knoten **Sites** , und klicken Sie dann auf **Website hinzufügen**.
5. Geben Sie im Feld **Website Name** einen Namen für die IIS-Website ein (z. b. **Demosite**).
6. Geben Sie im Feld **physischer Pfad** den Pfad zum lokalen Ordner ein (oder suchen Sie danach) (z. b. **c:\demosite**).
7. Geben Sie im Feld **Port** die Portnummer ein, auf der die Website gehostet werden soll (z. b. **85**).

    > [!NOTE]
    > Die Standard Portnummern lauten 80 für http und 443 für HTTPS. Wenn Sie diese Website jedoch auf Port 80 hosten, müssen Sie die Standard Website abbrechen, bevor Sie auf Ihre Website zugreifen können.
8. Lassen Sie das Feld **Hostname** leer, außer wenn Sie einen Domain Name System Eintrag (DNS) für die Website konfigurieren möchten, und klicken Sie dann auf **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image11.png)

    > [!NOTE]
    > In einer Produktionsumgebung möchten Sie wahrscheinlich Ihre Website auf Port 80 hosten und einen Host Header sowie übereinstimmende DNS-Einträge konfigurieren. Weitere Informationen zum Konfigurieren von Host Headern in IIS 7 finden Sie unter [Konfigurieren eines Host Headers für eine Website (IIS 7)](https://technet.microsoft.com/library/cc753195(WS.10).aspx). Weitere Informationen zur DNS-Server Rolle in Windows Server finden Sie unter [DNS-Server Übersicht](https://technet.microsoft.com/library/cc770392.aspx) und [DNS-Server](https://technet.microsoft.com/windowsserver/dd448607).
9. Klicken Sie im **Aktions** Bereich unter **Website bearbeiten**auf **Bindungen**.
10. Klicken Sie im Dialogfeld **Site Bindungen** auf **Hinzufügen**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image12.png)
11. Legen Sie im Dialogfeld **Site Bindung hinzufügen** die **IP-Adresse** und den **Port** entsprechend der vorhandenen Standort Konfiguration fest.
12. Geben Sie im Feld **Hostname** den Namen des Webservers ein (z. b. **STAGEWEB1**), und klicken Sie dann auf **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image13.png)

    > [!NOTE]
    > Mit der ersten Site Bindung können Sie mithilfe der IP-Adresse und des Ports oder `http://localhost:85`lokal auf die Website zugreifen. Die zweite Site Bindung ermöglicht den Zugriff auf die Website von anderen Computern in der Domäne mithilfe des Computer namens (z. b. http://stageweb1:85).
13. Klicken Sie im Dialogfeld **Site Bindungen** auf **Schließen**.
14. Klicken Sie im Bereich **Verbindungen** auf **Anwendungs Pools**.
15. Klicken Sie im Bereich **Anwendungs Pools** mit der rechten Maustaste auf den Namen des Anwendungs Pools, und klicken Sie dann auf **Grundeinstellungen**. Standardmäßig entspricht der Name Ihres Anwendungs Pools dem Namen Ihrer Website (z. b. **Demosite**).
16. Wählen Sie in der Liste **.NET CLR-Version** die Option **.NET CLR v 4.0.30319**aus, und klicken Sie dann auf **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image21.png)

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

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image15.png)
5. Geben Sie im Dialogfeld **Benutzer oder Gruppen auswählen** **IIS\_ius RS**ein, klicken Sie auf **Namen überprüfen**, und klicken Sie dann auf **OK**.
6. Beachten Sie im Dialogfeld <strong>Berechtigungen für</strong><em>[Ordnername]</em> , dass der neuen Gruppe standardmäßig die Berechtigungen <strong>Lesen &amp; ausführen</strong>, <strong>Ordner Inhalt auflisten</strong>und <strong>Lesen</strong> zugewiesen wurden. Bleiben Sie unverändert, und klicken Sie auf <strong>OK</strong>.
7. Klicken Sie auf <strong>OK</strong> , um das Dialogfeld <em>[Ordnername]</em><strong>Eigenschaften</strong> zu schließen.

Als letzte Aufgabe müssen Sie dem nicht-Administrator Benutzer, dessen Anmelde Informationen Sie zum Bereitstellen von Inhalt verwenden, die entsprechenden Berechtigungen erteilen. Dieser Benutzer benötigt die Berechtigungen zum Remote Bereitstellen von Inhalten auf Ihrer Website.

**So konfigurieren Sie die IIS-Website Berechtigungen für einen nicht Administrator Domänen Benutzer**

1. Klicken Sie im IIS-Manager im Bereich **Verbindungen** mit der rechten Maustaste auf den Website Knoten (z. b. **Demosite**), zeigen **Sie auf bereit**stellen, und klicken Sie dann auf **Web deploy Veröffentlichung konfigurieren**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image16.png)
2. Klicken Sie im Dialogfeld **Web deploy Veröffentlichung konfigurieren** rechts neben der Liste **Benutzer zum Veröffentlichen von Veröffentlichungs Berechtigungen auswählen** auf die Schaltfläche mit den Auslassungs Punkten.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image17.png)
3. Geben Sie im Dialogfeld **Benutzer zulassen** die Domäne und den Benutzernamen des Kontos ein, das Sie zum Bereitstellen von Inhalt verwenden möchten, und klicken Sie dann auf **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image18.png)
4. Klicken Sie im Dialogfeld **Web deploy Veröffentlichung konfigurieren** auf **Setup**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image19.png)

    > [!NOTE]
    > Dieser Vorgang führt zwei wichtige Funktionen in einem Schritt aus. Zuerst wird dem Benutzer die Berechtigung zum Remote ändern der Website über den Webverwaltungsdienst erteilt, gemäß den Delegierungs Regeln, die Sie im vorherigen Abschnitt überprüft haben. Zweitens wird dem Benutzer die vollständige Kontrolle über den Quellordner für die Website gewährt, wodurch der Benutzerberechtigungen für den Website Inhalt hinzufügen, ändern und festlegen kann.
5. Klicken Sie im Dialogfeld **Web deploy Veröffentlichung konfigurieren** auf **Schließen**.

## <a name="configure-firewall-exceptions"></a>Konfigurieren von Firewallausnahmen

Standardmäßig lauscht der IIS-Webverwaltungsdienst an TCP-Port 8172. Wenn die Windows-Firewall auf dem Webserver aktiviert ist, müssen Sie eine neue eingehende Regel erstellen, um TCP-Datenverkehr an Port 8172 zuzulassen (der gesamte ausgehende Datenverkehr wird standardmäßig in der Windows-Firewall zugelassen). Wenn Sie eine Drittanbieter-Firewall verwenden, müssen Sie Regeln erstellen, um Datenverkehr zuzulassen.

| Direction | Von Port | An Port | Porttyp |
| --- | --- | --- | --- |
| Eingehend | Beliebig | 8172 | TCP |
| Ausgehend | 8172 | Beliebig | TCP |

Weitere Informationen zum Konfigurieren von Regeln in der Windows-Firewall finden Sie unter [Konfigurieren von Firewallregeln](https://technet.microsoft.com/library/dd448559(WS.10).aspx). Informationen zu Firewalls von Drittanbietern finden Sie in der Produktdokumentation.

## <a name="conclusion"></a>Zusammenfassung

Ihr Webserver sollte nun bereit sein, Remote Bereitstellungen für den Web deploy-Handler über den Webverwaltungsdienst zu akzeptieren. Bevor Sie versuchen, eine Webanwendung auf dem Server bereitzustellen, sollten Sie diese wichtigen Punkte überprüfen:

- Haben Sie die Standard Authentifizierung auf Serverebene in IIS aktiviert?
- Haben Sie Remote Verbindungen mit dem Webverwaltungsdienst aktiviert?
- Haben Sie den Webverwaltungsdienst gestartet?
- Sind Verwaltungsdienst-Delegierungs Regeln vorhanden?
- Hat die Anwendungs Pool Identität Lesezugriff auf den Quellordner für Ihre Website?
- Hat das Benutzerkonto ohne Administrator Berechtigungen auf Website Ebene in IIS Berechtigungen?
- Lässt Ihre Firewall eingehende Verbindungen mit dem Server über TCP-Port 8172 zu?

## <a name="further-reading"></a>Weitere nützliche Informationen

Eine Anleitung zum Konfigurieren von Projektdateien für benutzerdefinierte Microsoft-Build-Engine (MSBuild) zum Bereitstellen von Webpaketen für den Web deploy Handler finden Sie unter [Konfigurieren von Bereitstellungs Eigenschaften für eine Zielumgebung](configuring-deployment-properties-for-a-target-environment.md).

> [!div class="step-by-step"]
> [Zurück](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
> [Weiter](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
