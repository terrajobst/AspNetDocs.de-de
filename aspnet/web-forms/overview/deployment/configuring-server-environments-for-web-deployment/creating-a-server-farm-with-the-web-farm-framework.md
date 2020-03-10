---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework
title: Erstellen einer Server Farm mit dem Webfarm Framework | Microsoft-Dokumentation
author: jrjlee
description: In diesem Thema wird beschrieben, wie das Web Farm Framework (WFF) 2,0 verwendet wird, um eine Webserver Farm aus einer Sammlung von Servern zu erstellen und zu konfigurieren.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 656dd06d-806c-467c-863d-9fc45e5ba3ab
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework
msc.type: authoredcontent
ms.openlocfilehash: 204996514bed336e60ab77f184a923f04e7e2bba
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78517497"
---
# <a name="creating-a-server-farm-with-the-web-farm-framework"></a>Erstellen einer Serverfarm mit dem Webfarmframework

von [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird beschrieben, wie das Web Farm Framework (WFF) 2,0 verwendet wird, um eine Webserver Farm aus einer Sammlung von Servern zu erstellen und zu konfigurieren.

Mit WFF können Sie Webplattform-Produkte und-Komponenten, Webanwendungen, Websites und Konfigurationseinstellungen auf mehreren Webservern mit Lastenausgleich synchronisieren. In Szenarien, in denen Sie mehr als einen Webserver benötigen, wie etwa Staging-und Produktionsumgebungen, kann dies den Bereitstellungs-und Konfigurationsprozess erheblich vereinfachen. Sie können eine Webanwendung auf einem einzelnen Server&#x2014;auf dem *primären Server*&#x2014;bereitstellen, und WFF repliziert diese Webanwendung automatisch auf allen anderen Webservern in der Serverfarm.

## <a name="understanding-the-web-farm-framework"></a>Grundlegendes zum Webfarm-Framework

Sie können WFF 2,0 verwenden, um Inhalte für eine Gruppe von Webservern bereitzustellen, zu verwalten und bereitzustellen. Eine WFF-Bereitstellung besteht aus drei wichtigen Server Rollen:

- Der *Controller Server*. Sie verwenden diesen Server, um WFF-Serverfarmen zu erstellen und zu konfigurieren. Der Controller Server verwaltet die Synchronisierung von webplatt Form Komponenten, Konfigurationseinstellungen und Anwendungen zwischen den Webservern in einer Serverfarm. Sie installieren WFF 2,0 auf dem Controller Server, und der Controller Server installiert wiederum den WFF-Agent auf allen Servern in einer Serverfarm. Der Controller Server gehört nicht konzeptionell zu einer WFF-Serverfarm, und ein einzelner Controller Server kann mehrere Serverfarmen verwalten. In diesem Szenario verwenden Sie einen einzelnen WFF-Controller Server, um die stagingserverfarm und die Produktionsserver Farm zu erstellen und zu verwalten.
- Der *primäre Server*. Jede WFF-Serverfarm enthält einen einzelnen primären Server. Wenn Sie webplatt Form Komponenten installieren oder Anwendungen auf dem primären Server bereitstellen, synchronisiert das WFF die Änderungen mit allen anderen Servern in der Serverfarm.
- Der *sekundäre Server*. Jede WFF-Serverfarm umfasst mindestens einen sekundären Server. Alle Änderungen, die Sie am primären Server vornehmen, werden auf jeden sekundären Server innerhalb der Serverfarm repliziert.

Dies zeigt die Beziehung zwischen diesen Server Rollen und den Staging-und Produktionsumgebungen von Fabrikam, Inc.:

![](creating-a-server-farm-with-the-web-farm-framework/_static/image1.png)

In diesem Szenario sind die Stagingumgebung und die Produktionsumgebung beide als WFF-Serverfarmen konfiguriert. Ein einzelner WFF-Controller Server verwaltet beide Farmen. In jeder Serverfarm werden alle Änderungen am primären Server auf jedem sekundären Server repliziert.

Bevor Sie mit der Konfiguration der Staging-und Produktionsumgebungen beginnen, sollten Sie diese Artikel lesen, um sich mit den wichtigsten Konzepten von WFF 2,0 vertraut zu machen:

- [Übersicht über das Web Farm Framework 2,0 für IIS 7](https://go.microsoft.com/?linkid=9805126)
- [Einrichten einer Server Farm mit dem Webfarm-Framework 2,0 für IIS 7](https://go.microsoft.com/?linkid=9805127)
- [System-und Platt Form Anforderungen für das Web Farm Framework 2,0 für IIS 7](https://go.microsoft.com/?linkid=9805128)

## <a name="task-overview"></a>Aufgaben Übersicht

Um die Aufgaben und exemplarischen Vorgehensweisen in diesem Thema abzuschließen, benötigen Sie mindestens drei Server&#x2014;, einen WFF-Controller, einen primären Webserver für die Serverfarm und einen oder mehrere sekundäre Webserver für die Serverfarm. Sie können einer WFF-Serverfarm jederzeit weitere sekundäre Server hinzufügen. Auf hoher Ebene müssen Sie Folgendes tun, um eine WFF-Serverfarm für ihre Staging-oder Produktionsumgebung zu erstellen und zu konfigurieren:

- Erstellen Sie einen Controller Server, indem Sie Internetinformationsdienste (IIS) 7,5 und WFF 2,0 installieren.
- Bereiten Sie primäre und sekundäre Server vor, indem Sie ein gemeinsames Administrator Konto erstellen und Firewallausnahmen konfigurieren.
- Konfigurieren Sie die Serverfarm mithilfe von IIS-Manager auf dem Controller Server.
- Konfigurieren Sie den Lastenausgleich mithilfe von IIS Application Request Routing (arr) oder einer alternativen Lasten Ausgleichs Technologie.

Bei den Aufgaben und exemplarischen Vorgehensweisen in diesem Thema wird davon ausgegangen, dass Sie mit Clean Server Builds beginnen, die Windows Server 2008 R2 ausführen. Bevor Sie beginnen, stellen Sie für jeden Server Folgendes sicher:

- Windows Server 2008 R2 Service Pack 1 und alle verfügbaren Updates werden installiert.
- Der Server ist einer Domäne beigetreten.
- Der Server verfügt über eine statische IP-Adresse.

> [!NOTE]
> Weitere Informationen zum Hinzufügen von Computern zu einer Domäne finden Sie unter [Hinzufügen von Computern zur Domäne und anmelden](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Weitere Informationen zum Konfigurieren statischer IP-Adressen finden Sie unter [Konfigurieren einer statischen IP-Adresse](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx).

## <a name="create-the-wff-controller-server"></a>Erstellen des WFF-Controller Servers

Zum Erstellen eines WFF-Controller Servers müssen Sie IIS 7 oder höher und WFF 2,0 oder höher installieren. Im Untertitel verwendet WFF das IIS-Webbereitstellungs Tool (Web deploy) 2. x, um die Server in der Farm zu synchronisieren. Wenn Sie den Webplattform-Installer zum Installieren von WFF verwenden, lädt der Installer automatisch Web deploy für Sie herunter und installiert sie.

**So erstellen Sie den WFF-Controller Server**

1. Herunterladen und Installieren des [Webplattform-Installers](https://go.microsoft.com/?linkid=9739157).
2. Klicken Sie oben im Fenster **Webplattform-Installer 3,0** auf **Produkte**.
3. Klicken Sie auf der linken Seite des Fensters im Navigationsbereich auf **Server**.
4. Klicken Sie in der Zeile mit der **empfohlenen IIS 7-Konfiguration** auf **Hinzufügen**.
5. Im <strong>Webfarm Framework 2.</strong> <em>x</em> Zeile, klicken Sie auf <strong>Hinzufügen</strong>.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image2.png)
6. Klicken Sie auf **Installieren**. Beachten Sie, dass der Webplattform-Installer der Installationsliste das Webbereitstellungs Tool zusammen mit verschiedenen anderen Abhängigkeiten hinzugefügt hat.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image3.png)
7. Lesen Sie die Lizenzbedingungen, und klicken Sie auf **akzeptieren**, wenn Sie den Bedingungen zustimmen.
8. Wenn die Installation abgeschlossen ist, klicken Sie auf **Fertig**stellen, und schließen Sie dann das Fenster **Webplattform-Installer 3,0** .

## <a name="configure-the-primary-and-secondary-servers"></a>Konfigurieren der primären und sekundären Server

Bevor Sie eine WFF-Serverfarm erstellen, sollten Sie einige Vorbereitungsaufgaben auf den Webservern ausführen, die die Farm bilden:

- Fügen Sie Firewallausnahmen hinzu, um die Kommunikation zwischen den Funktionen **Netzwerk**, **Remote Verwaltung**und **Datei-und Druckerfreigabe** mit dem WFF-Controller Server zuzulassen.
- Erstellen Sie in Active Directory ein Domänen Konto (z. b. **fabrikam\stagingfarm**), und fügen Sie es der lokalen Gruppe Administratoren auf jedem Server hinzu. Sie verwenden dieses Konto als Administrator Konto für die Serverfarm, wenn Sie die Serverfarm erstellen.

Weitere Informationen zum Konfigurieren dieser Firewallausnahmen in der Windows-Firewall finden Sie unter [System-und Platt Form Anforderungen für das Web Farm Framework 2,0 für IIS 7](https://go.microsoft.com/?linkid=9805128). Informationen zu anderen Firewallsystemen finden Sie in der Produktdokumentation.

Sie können das nächste Verfahren verwenden, um der lokalen Administrator Gruppe in Windows Server 2008 R2 ein Domänen Konto hinzuzufügen. Führen Sie diese Schritte auf jedem Server aus, den Sie der Serverfarm&#x2014;hinzufügen möchten. Fügen Sie der lokalen Gruppe Administratoren auf dem primären Server und auf jedem sekundären Server das gleiche Domänen Konto hinzu.

**So fügen Sie der lokalen Gruppe "Administratoren" ein Domänen Konto hinzu**

1. Zeigen Sie im **Startmenü** auf **Verwaltung**, und klicken Sie dann auf **Server-Manager**.
2. Erweitern Sie im Fenster **Server-Manager** im Struktur Ansichts Bereich **Konfiguration**, erweitern Sie **lokale Benutzer und Gruppen**, und klicken Sie dann auf **Gruppen**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image4.png)
3. Doppelklicken Sie im Bereich **Gruppen** auf **Administratoren**.
4. Klicken Sie im Dialogfeld **Administrator Eigenschaften** auf **Hinzufügen**.
5. Geben Sie im Dialogfeld **Benutzer, Computer, Dienst Konten oder Gruppen auswählen** das Domänen Konto (oder durchsuchen) (z. b. **fabrikam\stagingfarm**) ein, und klicken Sie dann auf **OK**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image5.png)
6. Klicken Sie im Dialogfeld **Administrator Eigenschaften** auf **OK**.

Ihre Server können nun einer Serverfarm hinzugefügt werden. Im Fall des primären Servers können Sie den Server so konfigurieren, dass er die Anwendungsanforderungen erfüllt, bevor oder nachdem Sie die Serverfarm&#x2014;in beiden Fällen erstellt haben. das WFF synchronisiert die Server, indem die gleichen Produkte, Komponenten oder Konfigurationen auf den sekundären Servern bereitgestellt werden. Der Einfachheit halber wird in diesem Tutorial davon ausgegangen, dass Sie den primären Server konfigurieren, wenn Sie die Serverfarm erstellt haben.

## <a name="create-the-wff-server-farm"></a>Erstellen der WFF-Server Farm

An diesem Punkt können alle Ihre Server einer WFF-Serverfarm hinzugefügt werden:

- Sie haben WFF auf dem Controller Server installiert.
- Sie haben Firewallausnahmen auf den primären und sekundären Webservern konfiguriert.
- Sie haben der lokalen Gruppe "Administratoren" auf Ihren primären und sekundären Webservern ein Domänen Konto hinzugefügt.

Der nächste Schritt ist das Erstellen der Serverfarm in WFF. Dies können Sie über den IIS-Manager auf dem WFF Controller-Server tun.

**So erstellen Sie eine WFF-Serverfarm**

1. Zeigen Sie auf dem WFF-Controller Server im **Startmenü** auf **Verwaltung**, und klicken Sie dann auf **Internetinformationsdienste (IIS)-Manager**.
2. Erweitern Sie im Bereich **Verbindungen** den Knoten lokaler Server, klicken Sie mit der rechten Maustaste auf **Serverfarmen**, und klicken Sie dann auf **Server Farm erstellen**.
3. Geben Sie im Dialogfeld **Server Farm erstellen** einen aussagekräftigen Namen für die Server Farm ein (z. b. Stagingfarm), und wählen Sie dann **Server Farm bereit**stellen aus.
4. Geben Sie den Benutzernamen und das Kennwort des Domänen Kontos ein, das Sie der lokalen Administrator Gruppe auf den einzelnen Servern hinzugefügt haben.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image6.png)
5. Klicken Sie auf **Weiter**.
6. Geben Sie auf der Seite **Server hinzufügen** den voll qualifizierten Domänen Namen (FQDN) des primären Servers ein, wählen Sie **primärer Server**aus, und klicken Sie dann auf **Hinzufügen**.
7. An diesem Punkt versucht WFF, den primären Server mit den von Ihnen angegebenen Anmelde Informationen zu kontaktieren. Wenn die Verbindung erfolgreich hergestellt wird, wird der primäre Server der Tabelle auf der Seite **Server hinzufügen** hinzugefügt.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image7.png)

    > [!NOTE]
    > Sie haben möglicherweise bemerkt, dass der **Server für den Lastenausgleich verfügbar** ist. ist standardmäßig ausgewählt. WFF verwendet das IIS arr-Modul, um den Lastenausgleich zu implementieren und damit Anforderungen über die Webserver in der Serverfarm zu verteilen. In den meisten Szenarien löschen Sie nur die Option **Server ist für den Lastenausgleich verfügbar** , wenn Sie stattdessen eine Lasten Ausgleichs Lösung eines Drittanbieters verwenden möchten.
8. Geben Sie auf der Seite **Server hinzufügen** den voll qualifizierten Namen des ersten sekundären Servers ein, und klicken Sie dann auf **Hinzufügen**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image8.png)
9. Wiederholen Sie Schritt 7 für alle zusätzlichen sekundären Server in der Farm, und klicken Sie dann auf **Fertig**stellen.

Ihre WFF-Serverfarm ist nun in Betrieb. Alle Webplattform-Produkte oder-Komponenten, die Sie auf dem primären Server installieren, sowie alle Webanwendungen oder-Inhalte, die Sie auf dem primären Server bereitstellen, werden automatisch auf allen sekundären Servern bereitgestellt.

WFF ist ein umfassendes und komplexes Thema. Weitere Informationen hierzu finden Sie auf der Website [Microsoft Web Farm Framework 2,0 for IIS 7](https://go.microsoft.com/?linkid=9805129) . Es gibt jedoch zwei Featurebereiche, die Sie beachten sollten:

- Die *Anwendungs Bereitstellung* ist der Prozess, bei dem Inhalte vom primären Server wie Webanwendungen und Konfigurationseinstellungen auf allen sekundären Servern in der Serverfarm repliziert werden. Wenn Sie z. b. die Kontakt-Manager-Beispiellösung auf dem primären Stagingserver bereitstellen, wird diese Lösung vom WFF-Anwendungs Bereitstellungs Prozess auf allen sekundären Stagingservern bereitgestellt. Der Anwendungs Bereitstellungs Prozess wird standardmäßig alle 30 Sekunden ausgeführt.
- Die *Platt Form Bereitstellung* ist der Prozess, bei dem Webplattform-Produkte und-Komponenten vom primären Server auf alle sekundären Server in der Serverfarm synchronisiert werden. Wenn Sie z. b. ASP.NET MVC 3 auf Ihrem primären Stagingserver installieren, verwendet der Platt Form Bereitstellungs Prozess den Webplattform-Installer, um ASP.NET MVC 3 auf allen sekundären Stagingservern zu installieren. Standardmäßig wird der Platt Form Bereitstellungs Prozess alle fünf Minuten ausgeführt.

Sie können die grundlegenden Einstellungen für die Anwendungs-und Platt Form Bereitstellung im IIS-Manager auf dem WFF Controller-Server verwalten.

**Anwendungs-und Platt Form Bereitstellungs Einstellungen erkunden**

1. Wählen Sie im IIS-Manager im Bereich **Verbindungen** die Serverfarm aus.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image9.png)
2. Doppelklicken Sie im Bereich **Server Farm** auf **Anwendungs Bereitstellung**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image10.png)
3. Wie Sie sehen können, ist die Serverfarm zurzeit so konfiguriert, dass Webinhalts-und Konfigurationseinstellungen zwischen dem primären Server und den sekundären Servern alle 30 Sekunden synchronisiert werden.
4. Klicken Sie auf **zurück**, und doppelklicken Sie dann auf **Platt Form Bereitstellung**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image11.png)
5. Wie Sie sehen, ist die Serverfarm zurzeit so konfiguriert, dass Webplattform-Produkte und-Komponenten zwischen dem primären Server und den sekundären Servern alle fünf Minuten synchronisiert werden.
6. Klicken Sie auf **Zurück**.
7. Um zu erzwingen, dass die Serverfarm Webplattform-Produkte sofort synchronisiert, klicken Sie im **Aktions** Bereich auf **Plattform bereit**stellen.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image12.png)

    > [!NOTE]
    > Die Platt Form Bereitstellung kann einige Zeit in Anspruch nehmen. Der Installationsprogramm Prozess wird im Hintergrund auf den sekundären Servern in der Serverfarm ausgeführt.
8. Nachdem Sie genügend Zeit für den Abschluss des Bereitstellungs Vorgangs erhalten haben, können Sie überprüfen, ob die Produkte und Komponenten, die Sie dem primären Server hinzugefügt haben, nun auf den sekundären Servern repliziert wurden. Beispielsweise können Sie sich an einem sekundären Server anmelden und das **Server-Manager** Fenster verwenden, um zu überprüfen, ob die Webserver Rolle installiert wurde.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image13.png)
9. Sie können auch die Liste der installierten Programme überprüfen, um zu überprüfen, ob verschiedene Webplattform-Komponenten hinzugefügt wurden.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image14.png)

## <a name="configure-load-balancing"></a>Konfigurieren des Lasten Ausgleichs

Wenn Sie eine Webfarm erstellen, müssen Sie eine Art von Lastenausgleich einrichten, um HTTP-Anforderungen zwischen ihren Webservern zu verteilen. Dabei kann es sich um den Netzwerk Lastenausgleich von Windows Server 2008, IIS ARR oder eine softwarebasierte oder hardwarebasierte Lösung für den Lastenausgleich von Drittanbietern handeln.

WFF ist für die enge Integration in IIS arr konzipiert. Um diese Integration nutzen zu können, müssen Sie das arr-Modul auf dem WFF-Controller Server installieren. Anschließend leiten Sie den gesamten Webdatenverkehr an den Controller Server weiter, indem Sie in der Regel Domain Name System (DNS)-Einträge konfigurieren. Der Controller Server verteilt dann eingehende Anforderungen auf Grundlage der Serververfügbarkeit und verschiedener anderer Kriterien auf die Server in der Farm.

> [!NOTE]
> Sie müssen arr nicht mit WFF verwenden. Sie können WFF so konfigurieren, dass es mit Lasten Ausgleichs Lösungen von Drittanbietern funktioniert. Weitere Informationen finden Sie unter [Übersicht über das Web Farm Framework 2,0 für IIS 7](https://go.microsoft.com/?linkid=9805126).

Der Lastenausgleich mithilfe von arr ist ein komplexes Thema, das größtenteils den Rahmen dieses Tutorials sprengen würde. Sie können jedoch das nächste Verfahren verwenden, um das arr-Modul zu installieren und mit dem Lastenausgleich zu beginnen.

**So richten Sie den Lastenausgleich auf dem WFF-Controller Server ein**

1. Starten Sie auf dem WFF-Controller Server den Webplattform-Installer.
2. Klicken Sie oben im Fenster **Webplattform-Installer 3,0** auf **Produkte**.
3. Klicken Sie auf der linken Seite des Fensters im Navigationsbereich auf **Server**.
4. Klicken Sie in der Zeile **Anwendungs Anforderungs Routing 2,5** auf **Hinzufügen**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image15.png)
5. Klicken Sie auf **Installieren**, und befolgen Sie dann die Anweisungen im Fenster **Webplattform-Installation** .
6. Starten Sie nach Abschluss der Installation den IIS-Manager, und klicken Sie im Bereich **Verbindungen** auf Ihren Serverfarm Knoten. Beachten Sie, dass dem Bereich **Server Farm** mehrere neue Symbole hinzugefügt wurden.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image16.png)
7. Doppelklicken Sie im Bereich **Server Farm** auf **Lastenausgleich**.
8. Wählen Sie im Bereich **Lastenausgleich** einen Lasten Ausgleichs Algorithmus (z. b. die niedrigste **aktuelle Anforderung**) aus.

    > [!NOTE]
    > Weitere Informationen zu Lasten Ausgleichs Algorithmen und anderen Konfigurationseinstellungen finden Sie unter [Application Request Routing Module](https://go.microsoft.com/?linkid=9805130).

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image17.png)
9. Klicken Sie im Bereich **Aktionen** auf **über**nehmen.

Sie haben jetzt einen grundlegenden Lastenausgleich für die Server in der Serverfarm konfiguriert. Wenn Sie den gesamten Webfarm-Datenverkehr an den Controller Server weiterleiten, werden die Anforderungen gemäß Verfügbarkeit und Lasten Ausgleichs Algorithmus, die Sie ausgewählt haben, auf die Server in der Farm verteilt.

Weitere Informationen zum Konfigurieren des Lasten Ausgleichs mit arr finden Sie unter [Application Request Routing Module](https://go.microsoft.com/?linkid=9805130).

## <a name="monitor-the-server-farm"></a>Überwachen der Server Farm

Sie können die Integrität Ihrer Serverfarm jederzeit über den IIS-Manager auf dem Controller Server überwachen. Erweitern Sie im Bereich **Verbindungen** die Serverfarm, und klicken Sie dann auf **Server**. Im mittleren Bereich wird eine Zusammenfassung der einzelnen Server in der Farm zusammen mit einem Ablauf Verfolgungs Protokoll der aktuellen Aktivitäten angezeigt.

![](creating-a-server-farm-with-the-web-farm-framework/_static/image18.png)

## <a name="conclusion"></a>Zusammenfassung

Ihre WFF-Serverfarm sollte nun in Betrieb sein. Sie können den primären Server so konfigurieren, dass ein beliebiges Bereitstellungs&#x2014;Verfahren unterstützt wird. weitere&#x2014;Informationen finden Sie im Abschnitt Weitere Informationen, und die Konfiguration wird auf jedem sekundären Server in der Serverfarm repliziert.

## <a name="further-reading"></a>Weitere nützliche Informationen

Weitere Informationen zu allen Aspekten der Konfiguration und Verwendung von WFF finden Sie auf der Website [Microsoft Web Farm Framework 2,0 for IIS 7](https://go.microsoft.com/?linkid=9805129) .

> [!div class="step-by-step"]
> [Zurück](configuring-a-database-server-for-web-deploy-publishing.md)
> [Weiter](configuring-deployment-properties-for-a-target-environment.md)
