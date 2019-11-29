---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
title: Einmaliges Anmelden (entwickeln realer Cloud-apps mit Azure) | Microsoft-Dokumentation
author: MikeWasson
description: Das e-Book zur Entwicklung realer Cloud-apps mit Azure basiert auf einer Präsentation von Scott Guthrie. Es werden 13 Muster und Vorgehensweisen erläutert, für die er...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 7d82d5e9-0619-4f22-9e03-32a6d52940a5
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
msc.type: authoredcontent
ms.openlocfilehash: 7e32f444dc38132296cffd45ac658f5abf51f314
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74585282"
---
# <a name="single-sign-on-building-real-world-cloud-apps-with-azure"></a>Einmaliges Anmelden (entwickeln realer Cloud-apps mit Azure)

von [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Herunterladen des IT-Projekts](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) oder [herunterladen des E-Books](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Das e-Book zur Entwicklung **realer Cloud-apps mit Azure** basiert auf einer Präsentation von Scott Guthrie. Es werden 13 Muster und Verfahren erläutert, die Ihnen bei der Entwicklung von Web-Apps für die Cloud helfen können. Informationen zum e-Book finden Sie [im ersten Kapitel](introduction.md).

Bei der Entwicklung einer Cloud-App gibt es viele Sicherheitsaspekte, die Sie berücksichtigen sollten, aber für diese Reihe konzentrieren wir uns auf nur einen: Single Sign-on. Eine Frage, die häufig gestellt wird, lautet: "Ich erstelle hauptsächlich Apps für die Mitarbeiter meines Unternehmens. wie hosten Sie diese apps in der Cloud und ermöglichen es Ihnen, dasselbe Sicherheitsmodell zu verwenden, das meine Mitarbeiter kennen und in der lokalen Umgebung verwenden, wenn Sie apps ausführen, die in der Firewall gehostet werden? " Eine der Möglichkeiten, dieses Szenario zu aktivieren, wird Azure Active Directory (Azure AD) genannt. Azure AD ermöglicht es Ihnen, Lob-Apps (Line-of-Business) über das Internet verfügbar zu machen, und ermöglicht es Ihnen, diese apps auch für Geschäftspartner verfügbar zu machen.

## <a name="introduction-to-azure-ad"></a>Einführung in Azure AD

[Azure AD](https://docs.microsoft.com/azure/active-directory/) [Active Directory](https://msdn.microsoft.com/library/windows/desktop/aa746492.aspx) in der Cloud bereit. Zu den wichtigsten Features zählen die folgenden:

- Es ist in lokale Active Directory integriert.
- Sie ermöglicht die Single Sign-on mit ihren apps.
- Es unterstützt offene Standards wie z. b. [SAML](http://en.wikipedia.org/wiki/SAML_2.0), [WS-Fed](http://en.wikipedia.org/wiki/WS-Federation)und [OAuth 2,0](http://oauth.net/2/).
- Es unterstützt die Enterprise [Graph-Rest-API](https://msdn.microsoft.com/library/hh974476.aspx).

Angenommen, Sie verfügen über eine lokale Windows Server-Active Directory Umgebung, die Sie verwenden, um Mitarbeitern das Anmelden bei intranetbasierten apps zu ermöglichen:

![](single-sign-on/_static/image1.png)

Mit Azure AD können Sie ein Verzeichnis in der Cloud erstellen. Es handelt sich um ein Kostenloses Feature, das leicht einzurichten ist.

Sie kann ganz unabhängig von Ihrer lokalen Active Directory sein; Sie können jede gewünschte Person in Sie einfügen und in Internet-apps authentifizieren.

![Windows Azure Active Directory gewähren](single-sign-on/_static/image2.png)

Oder Sie können Sie in das lokale Ad integrieren.

![AD-und Waad-Integration](single-sign-on/_static/image3.png)

Nun können sich alle Mitarbeiter, die sich lokal authentifizieren können, auch über das Internet authentifizieren – ohne dass Sie eine Firewall öffnen oder neue Server in Ihrem Rechenzentrum bereitstellen müssen. Sie können weiterhin die vorhandene Active Directory Umgebung nutzen, die Sie bereits kennen und heute verwenden, um Ihren internen Apps die Funktion für einmaliges Anmelden zu gewähren.

Nachdem Sie diese Verbindung zwischen AD und Azure AD hergestellt haben, können Sie Ihren Web-Apps und ihren mobilen Geräten auch ermöglichen, Ihre Mitarbeiter in der Cloud zu authentifizieren, und Sie können Drittanbieter-apps wie Office 365, Salesforce.com oder Google Apps aktivieren, um Ihre Anmelde Informationen des Mitarbeiters. Wenn Sie Office 365 verwenden, sind Sie bereits mit Azure AD eingerichtet, da Office 365 Azure AD für die Authentifizierung und Autorisierung verwendet.

![Drittanbieter-apps](single-sign-on/_static/image4.png)

Der Vorteil dieses Ansatzes besteht darin, dass Sie jedes Mal, wenn Ihr Unternehmen einen Benutzer hinzufügt oder löscht oder wenn ein Benutzer ein Kennwort ändert, den gleichen Prozess verwenden, den Sie auch heute in Ihrer lokalen Umgebung verwenden. Alle Ihre lokalen AD-Änderungen werden automatisch an die cloudumgebung weitergegeben.

Wenn Ihr Unternehmen Office 365 verwendet oder zu diesem wechselt, ist die gute Nachricht, dass Sie Azure AD automatisch einrichten, da Office 365 Azure AD für die Authentifizierung verwendet. So können Sie problemlos in ihren eigenen Apps die Authentifizierung verwenden, die von Office 365 verwendet wird.

## <a name="set-up-an-azure-ad-tenant"></a>Einrichten eines Azure AD Mandanten

ein Azure AD [Verzeichnis wird als Azure AD Mandanten](https://technet.microsoft.com/library/jj573650.aspx)bezeichnet, und das Einrichten eines Mandanten ist recht einfach. Wir zeigen Ihnen, wie es im Azure-Verwaltungsportal durchgeführt wird, um die Konzepte zu veranschaulichen, aber natürlich können Sie auch die anderen Portal Funktionen verwenden, indem Sie ein Skript oder eine Verwaltungs-API verwenden.

Klicken Sie im Verwaltungs Portal auf die Registerkarte Active Directory.

![Waad im Portal](single-sign-on/_static/image5.png)

Sie verfügen automatisch über einen Azure AD Mandanten für Ihr Azure-Konto, und Sie können unten auf der Seite auf die Schaltfläche **Hinzufügen** klicken, um weitere Verzeichnisse zu erstellen. Sie sollten z. b. eine für eine Testumgebung und eine für die Produktion benötigen. Überlegen Sie genau, wie Sie ein neues Verzeichnis benennen. Wenn Sie Ihren Namen für das Verzeichnis verwenden und dann Ihren Namen für einen der Benutzer wieder verwenden, kann dies verwirrend sein.

![Hinzufügen von Verzeichnissen](single-sign-on/_static/image6.png)

Das Portal bietet vollständige Unterstützung für das Erstellen, löschen und Verwalten von Benutzern in dieser Umgebung. Um beispielsweise einen Benutzer hinzuzufügen, wechseln Sie zur Registerkarte **Benutzer** , und klicken Sie auf die Schaltfläche **Benutzer hinzufügen** .

![Schaltfläche „Benutzer hinzufügen“](single-sign-on/_static/image7.png)

![Dialogfeld Benutzer hinzufügen](single-sign-on/_static/image8.png)

Sie können einen neuen Benutzer erstellen, der nur in diesem Verzeichnis vorhanden ist, oder Sie können ein Microsoft-Konto als Benutzer in diesem Verzeichnis registrieren oder einen Benutzer in einem anderen Azure AD Verzeichnis als Benutzer in diesem Verzeichnis registrieren. (In einem echten Verzeichnis ist die Standard Domäne ContosoTest.onmicrosoft.com. Sie können auch eine eigene Domäne verwenden, wie z. b. contoso.com.)

![Benutzer Typen](single-sign-on/_static/image9.png)

![Dialogfeld Benutzer hinzufügen](single-sign-on/_static/image10.png)

Sie können den Benutzer einer Rolle zuweisen.

![Benutzerprofil](single-sign-on/_static/image11.png)

Das Konto wird mit einem temporären Kennwort erstellt.

![Temporäres Kennwort](single-sign-on/_static/image12.png)

Die Benutzer, die Sie auf diese Weise erstellen, können sich sofort bei Ihren Web-Apps mit diesem cloudverzeichnis anmelden.

Was für Enterprise Single Sign-on hervorragend ist, ist jedoch die Registerkarte **Verzeichnis Integration** :

![Registerkarte Verzeichnis Integration](single-sign-on/_static/image13.png)

Wenn Sie die Verzeichnisintegration aktivieren und [ein Tool herunterladen](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx), können Sie dieses cloudverzeichnis mit Ihrem vorhandenen lokalen Active Directory synchronisieren, das Sie bereits in Ihrer Organisation verwenden. Anschließend werden alle in Ihrem Verzeichnis gespeicherten Benutzer in diesem cloudverzeichnis angezeigt. Ihre Cloud-Apps können jetzt alle Ihre Mitarbeiter mithilfe Ihrer vorhandenen Active Directory Anmelde Informationen authentifizieren. Und das ist kostenlos – sowohl das Synchronisierungs Tool als auch Azure AD selbst.

Das Tool ist ein Assistent, der einfach zu verwenden ist, wie Sie in diesen Bildschirmfotos sehen können. Dies sind keine umfassenden Anweisungen, sondern nur ein Beispiel, das den grundlegenden Prozess veranschaulicht. Ausführlichere Informationen zur Vorgehensweise finden Sie unter den Links im Abschnitt " [Ressourcen](#resources) " am Ende des Kapitels.

![Assistent zum Konfigurieren von Waad-Synchronisierungs Tools](single-sign-on/_static/image14.png)

Klicken Sie auf **weiter**, und geben Sie Ihre Azure Active Directory Anmelde Informationen ein.

![Assistent zum Konfigurieren von Waad-Synchronisierungs Tools](single-sign-on/_static/image15.png)

Klicken Sie auf **weiter**, und geben Sie Ihre lokalen AD-Anmelde Informationen ein.

![Assistent zum Konfigurieren von Waad-Synchronisierungs Tools](single-sign-on/_static/image16.png)

Klicken Sie auf **weiter**, und geben Sie dann an, ob Sie einen Hash Ihrer AD-Kenn Wörter in der Cloud speichern möchten.

![Assistent zum Konfigurieren von Waad-Synchronisierungs Tools](single-sign-on/_static/image17.png)

Der Kenn Wort Hash, den Sie in der Cloud speichern können, ist ein unidirektionaler Hash. tatsächliche Kenn Wörter werden niemals in Azure AD gespeichert. Wenn Sie sich gegen die Speicherung von Hashes in der Cloud entscheiden, müssen Sie [Active Directory-Verbunddienste (AD FS)](https://technet.microsoft.com/library/hh831502.aspx) (ADFS) verwenden. Es gibt auch [andere Faktoren, die Sie berücksichtigen sollten, wenn Sie entscheiden, ob Sie AD FS verwenden möchten](https://technet.microsoft.com/library/jj573653.aspx). Die Option ADFS erfordert einige zusätzliche Konfigurationsschritte.

Wenn Sie Hashes in der Cloud speichern möchten, sind Sie fertig, und das Tool beginnt mit der Synchronisierung von Verzeichnissen, wenn Sie auf **weiter**klicken.

![Assistent zum Konfigurieren von Waad-Synchronisierungs Tools](single-sign-on/_static/image18.png)

Und in wenigen Minuten sind Sie fertig.

![Assistent zum Konfigurieren von Waad-Synchronisierungs Tools](single-sign-on/_static/image19.png)

Sie müssen dies nur auf einem Domänen Controller in der Organisation unter Windows 2003 oder höher ausführen. Und es ist kein Neustart erforderlich. Wenn Sie fertig sind, befinden sich alle Ihre Benutzer in der Cloud, und Sie können Single Sign-on über eine beliebige Web-oder Mobile Anwendung ausführen, indem Sie SAML, OAuth oder WS-Fed verwenden.

Manchmal werden wir gefragt, wie sicher dies ist – verwendet Microsoft es für Ihre eigenen sensiblen Geschäftsdaten? Die Antwort lautet ja. Wenn Sie z. b. auf [https://microsoft.sharepoint.com/](https://microsoft.sharepoint.com/)zur internen Microsoft SharePoint-Website wechseln, werden Sie aufgefordert, sich anzumelden.

![Office 365-Anmeldung](single-sign-on/_static/image20.png)

Microsoft hat AD FS aktiviert. Wenn Sie also eine Microsoft-ID eingeben, werden Sie auf eine AD FS-Anmeldeseite umgeleitet.

![AD FS-Anmeldung](single-sign-on/_static/image21.png)

Nachdem Sie Anmelde Informationen eingegeben haben, die in einem internen Microsoft AD-Konto gespeichert sind, haben Sie Zugriff auf diese interne Anwendung.

![MS SharePoint-Website](single-sign-on/_static/image22.png)

Wir verwenden einen AD-Anmelde Server vor allem, weil bereits ADFS eingerichtet war, bevor Azure AD verfügbar wurden, aber der Anmeldevorgang durchläuft ein Azure AD Verzeichnis in der Cloud. Wir platzieren unsere wichtigen Dokumente, Quell Code Verwaltung, Leistungs Verwaltungs Dateien, Verkaufsberichte und vieles mehr in der Cloud und verwenden diese exakt gleiche Lösung, um Sie zu sichern.

## <a name="create-an-aspnet-app-that-uses-azure-ad-for-single-sign-on"></a>Erstellen einer ASP.net-APP, die Azure AD für verwendet Single Sign-on

Visual Studio macht es ganz einfach, eine APP zu erstellen, die Azure AD für Single Sign-on verwendet, wie Sie in einigen Screenshots sehen können.

Wenn Sie eine neue ASP.NET-Anwendung erstellen, entweder MVC oder Web Forms, wird die Standard Authentifizierungsmethode ASP.net Identity. Um das Azure AD zu ändern, klicken Sie auf die Schaltfläche **Authentifizierung ändern** .

![Authentifizierung ändern](single-sign-on/_static/image23.png)

Wählen Sie Organisations Konten aus, geben Sie Ihren Domänen Namen ein, und wählen Sie dann einmaliges Anmelden aus.

![Dialogfeld zur Authentifizierung konfigurieren](single-sign-on/_static/image24.png)

Sie können der APP auch Lese-oder Lese-/Schreibberechtigungen für Verzeichnis Daten erteilen. Wenn Sie dies tun, können Sie die [Azure Graph-Rest-API](https://msdn.microsoft.com/library/windowsazure/hh974476.aspx) verwenden, um die Telefonnummer des Benutzers zu suchen, herauszufinden, ob Sie sich im Büro befinden, wann Sie sich zuletzt angemeldet haben, usw.

Das ist alles, was Sie tun müssen: Visual Studio fordert die Anmelde Informationen für einen Administrator für Ihren Azure AD Mandanten an und konfiguriert dann sowohl das Projekt als auch ihren Azure AD Mandanten für die neue Anwendung.

Wenn Sie das Projekt ausführen, wird eine Anmeldeseite angezeigt, und Sie können sich mit den Anmelde Informationen eines Benutzers in Ihrem Azure AD Verzeichnis anmelden.

![Organisations Kontoanmeldung](single-sign-on/_static/image25.png)

![Angemeldet](single-sign-on/_static/image26.png)

Wenn Sie die app in Azure bereitstellen, müssen Sie lediglich das Kontrollkästchen **Organisations Authentifizierung aktivieren aktivieren** , sodass Visual Studio die gesamte Konfiguration für Sie übernimmt.

![Web veröffentlichen](single-sign-on/_static/image27.png)

Diese Screenshots stammen aus einem vollständigen schrittweisen Tutorial, das zeigt, wie Sie eine APP erstellen, die Azure AD Authentifizierung verwendet: [entwickeln von ASP.net-apps mit Azure Active Directory](../../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md).

## <a name="summary"></a>Summary

In diesem Kapitel haben Sie festgestellt, dass Azure Active Directory, Visual Studio und ASP.NET das Einrichten von Single Sign-on in Internet Anwendungen für die Benutzer Ihrer Organisation erleichtern. Ihre Benutzer können sich bei Internet-apps mit denselben Anmelde Informationen anmelden, die Sie zum Anmelden mit Active Directory in Ihrem internen Netzwerk verwenden.

Im [nächsten Kapitel](data-storage-options.md) werden die für eine Cloud-App verfügbaren Datenspeicher Optionen erläutert.

<a id="resources"></a>
## <a name="resources"></a>Ressourcen

Weitere Informationen finden Sie unter:

- [Azure Active Directory-Dokumentation](https://docs.microsoft.com/azure/active-directory/). Portal Seite für Azure AD Dokumentation auf der windowsazure.com-Website. Schritt-für-Schritt-Tutorials finden Sie im Abschnitt " **entwickeln** ".
- [Azure-Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/). Die Portal Seite für die Dokumentation zu Multi-Factor Authentication in Azure.
- [Authentifizierungs Optionen für das Organisations Konto](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauthoptions). Erläuterung der Azure AD Authentifizierungs Optionen im Dialogfeld Visual Studio 2013-Dialog Felds "Neues Projekt".
- [Microsoft Patterns and Practices-Verbund Identitäts Muster](https://msdn.microsoft.com/library/dn589790.aspx).
- [HOWTO: Installieren Sie das Azure Active Directory-Synchronisierungs Tool](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx).
- [Active Directory-Verbunddienste (AD FS) 2,0-Inhalts](https://social.technet.microsoft.com/wiki/contents/articles/2735.ad-fs-2-0-content-map.aspx)Zuordnung. Links zur Dokumentation zu ADFS 2,0.
- [Rollenbasierte und ACL-basierte Autorisierung in einer Windows Azure AD-Anwendung](https://code.msdn.microsoft.com/Role-Based-and-ACL-Based-86ad71a1). Beispielanwendung.
- [Azure Active Directory Graph-API Blog](https://blogs.msdn.com/b/aadgraphteam/).
- [Access Control bei der BYOD-und Verzeichnis Integration in eine Hybrid-Identitäts Infrastruktur](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/PCIT-B213#fbid=). Tech Ed 2014 Session Video von gayana bagdasaryan.

> [!div class="step-by-step"]
> [Zurück](web-development-best-practices.md)
> [Weiter](data-storage-options.md)
