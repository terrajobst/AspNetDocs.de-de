---
uid: visual-studio/overview/2012/windows-azure-authentication
title: Windows Azure-Authentifizierung | Microsoft-Dokumentation
author: Rick-Anderson
description: Mit Microsoft ASP.NET Tools für Windows Azure Active Directory können Sie die Authentifizierung für Webanwendungen, die auf Windows Azure-Websites gehostet werden, auf einfache Weise aktivieren...
ms.author: riande
ms.date: 02/20/2013
ms.assetid: a3cef801-a54b-4ebd-93c3-55764e2e14b1
msc.legacyurl: /visual-studio/overview/2012/windows-azure-authentication
msc.type: authoredcontent
ms.openlocfilehash: ce98effe18dd739504fb0d5453bae8a46c3ba102
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449463"
---
# <a name="windows-azure-authentication"></a>Microsoft Azure-Authentifizierung

von [Rick Anderson](https://twitter.com/RickAndMSFT)

> Mit Microsoft ASP.NET Tools für Windows Azure Active Directory können Sie die Authentifizierung für Webanwendungen, die auf [Windows Azure](https://www.windowsazure.com/home/features/web-sites/)-Websites gehostet werden, einfach aktivieren. Sie können die Windows Azure-Authentifizierung verwenden, um Office 365-Benutzer von Ihrer Organisation aus zu authentifizieren, Unternehmenskonten, die von Ihrer lokalen Active Directory oder von Benutzern, die in ihrer eigenen benutzerdefinierten Windows Azure Active Directory Domäne erstellt wurden, synchronisiert werden. Durch Aktivieren der Windows Azure-Authentifizierung wird Ihre Anwendung für die Authentifizierung von Benutzern mithilfe eines einzelnen [Windows Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) -Mandanten konfiguriert.
>
> Das Windows Azure-Authentifizierungs Tool ASP.net wird für Webrollen in einem Cloud-Dienst nicht unterstützt. Wir planen dies jedoch in einer zukünftigen Version. [Windows Identity Foundation](https://msdn.microsoft.com/library/hh291066(v=VS.110).aspx) (WIF) wird in Windows Azure-Webrollen unterstützt.
>
> Ausführliche Informationen zum Einrichten der Synchronisierung zwischen Ihrem lokalen Active Directory und Ihrem Windows Azure Active Directory-Mandanten finden Sie [unter Verwenden von AD FS 2,0 zum Implementieren und Verwalten von Single Sign-on](https://technet.microsoft.com/library/jj205462.aspx).
>
> Windows Azure Active Directory ist derzeit als [kostenloser Vorschau Dienst](https://azure.microsoft.com/free/?WT.mc_id=A443DD604)verfügbar.

## <a name="requirements"></a>Anforderungen:

- Visual Studio 2012 oder [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express)
- Erweiterungen für [Webtools für Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282228&amp;clcid=0x409) oder [Webtools Erweiterungen für Visual Studio Express 2012](https://go.microsoft.com/fwlink/?LinkID=282231&amp;clcid=0x409)
- [Microsoft ASP.NET Tools für Windows Azure Active Directory – Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282306) oder [Microsoft ASP.NET Tools für Windows Azure Active Directory – Visual Studio Express 2012 für Web](https://go.microsoft.com/fwlink/?LinkId=282652)

## <a name="create-an-aspnet-web-application-with-visual-studio-2012"></a>Erstellen einer ASP.NET-Webanwendung mit Visual Studio 2012

Sie können jede Webanwendung mit Visual Studio 2012 erstellen. in diesem Tutorial wird die ASP.NET MVC-intranetvorlage verwendet.

1. Erstellen Sie eine neue ASP.NET MVC 4-Intranetanwendung, und übernehmen Sie alle Standardeinstellungen. (Hierbei muss es sich um einen in **tra** NET und nicht um ein **ter** NET-Projekt handeln.)
     ![](windows-azure-authentication/_static/image1.png)

## <a name="enable-window-azure-authentication-when-you-are-a-global-administrator-of-the-tenet"></a>Aktivieren Sie die Azure-Authentifizierung in Windows (wenn Sie ein globaler Administrator des Mandanten sind).

Wenn Sie über keinen vorhandenen Windows Azure Active Directory-Mandanten (z. b. über ein vorhandenes Office 365-Konto) verfügen, können Sie einen neuen Mandanten erstellen, indem Sie sich für ein [neues Windows Azure Active Directory-Konto](https://g.microsoftonline.com/0AX00en/5)registrieren.

1. Wählen Sie im Menü Projekt die Option **Windows Azure-Authentifizierung aktivieren**:

   ![](windows-azure-authentication/_static/image2.png)

2. Geben Sie die Domäne für Ihren Windows Azure Active Directory-Mandanten ein (z. b. contoso.onmicrosoft.com), und klicken Sie auf **aktivieren**:

![](windows-azure-authentication/_static/image3.png)

3. Melden Sie sich im Dialogfeld für die Webauthentifizierung als Administrator für Ihren Windows Azure Active Directory-Mandanten an:

   ![](windows-azure-authentication/_static/image4.png)

![](windows-azure-authentication/_static/image5.png)

## <a name="enable-window-azure-by-a-non-administrator-of-the-tenet"></a>Aktivieren von Windows Azure durch einen nicht Administrator des Mandanten

Wenn Sie keine globalen Administrator Rechte für Ihren Windows Azure Active Directory-Mandanten haben, können Sie das Kontrollkästchen für die Bereitstellung der Anwendung deaktivieren.

![](windows-azure-authentication/_static/image6.png)

Im Dialogfeld werden die **Domäne**, die **Anwendungs Prinzipal-ID** und die **Antwort-URL** angezeigt, die für die Bereitstellung der Anwendung mit einem Azure Active Directory Tenet erforderlich sind. Sie müssen diese Informationen an jemanden übergeben, der über ausreichende Berechtigungen zum Bereitstellen der Anwendung verfügt. Ausführliche Informationen zur Verwendung des Cmdlets zum manuellen Erstellen des Dienst Prinzipals finden Sie unter Gewusst[wie: Implementieren von Single Sign-on mit Windows Azure Active Directory ASP.NET-Anwendung](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) .
Nachdem die Anwendung erfolgreich bereitgestellt wurde, können Sie auf Weiter klicken, **um die Datei "Web. config" mit den ausgewählten Einstellungen zu aktualisieren**. Wenn Sie die Entwicklung der Anwendung fortsetzen möchten, während Sie auf die Bereitstellung wartet, können Sie auf Schließen klicken, **um die Einstellungen in der Projektdatei zu merken**. Wenn Sie das nächste Mal aufrufen von Windows Azure-Authentifizierung aktivieren und das Kontrollkästchen Bereitstellung deaktivieren, werden die gleichen Einstellungen angezeigt, und Sie können auf **weiter**klicken und dann auf **diese Einstellungen in Web. config anwenden**.

1. Warten Sie, bis Ihre Anwendung für die Windows Azure-Authentifizierung konfiguriert und mit Windows Azure Active Directory bereitgestellt wurde.
2. Nachdem die Windows Azure-Authentifizierung für Ihre Anwendung aktiviert wurde, klicken Sie auf **Schließen:**

    ![](windows-azure-authentication/_static/image7.png)
3. Drücken Sie F5, um die Anwendung auszuführen. Sie sollten automatisch auf die Anmeldeseite umgeleitet werden. Verwenden Sie für die Anmeldung bei der Anwendung die Anmelde Informationen für das Verzeichnis Grundsatz.

    ![](windows-azure-authentication/_static/image1.jpg)
4. Da Ihre Anwendung zurzeit ein selbst signiertes Test Zertifikat verwendet, wird im Browser eine Warnung angezeigt, dass das Zertifikat nicht von einer vertrauenswürdigen Zertifizierungsstelle ausgestellt wurde.

    Diese Warnung kann bei der lokalen Entwicklung problemlos ignoriert werden **, indem Sie auf diese Website weiter klicken:**

    ![](windows-azure-authentication/_static/image8.png)
5. Sie haben sich nun mit der Windows Azure-Authentifizierung erfolgreich bei Ihrer Anwendung angemeldet!

    ![](windows-azure-authentication/_static/image2.jpg)

Durch das Aktivieren der Windows Azure-Authentifizierung werden die folgenden Änderungen an Ihrer Anwendung vorgenommen:

- Dem Projekt wird eine Anti-Cross-Site Request Fälschung ([CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)))-Klasse ( *App\_start\antixsrfconfig.cs* ) hinzugefügt.
- Die `System.IdentityModel.Tokens.ValidatingIssuerNameRegistry` nuget-Pakete werden dem Projekt hinzugefügt.
- Windows Identity Foundation-Einstellungen in Ihrer Anwendung werden so konfiguriert, dass Sie Sicherheits Token von Ihrem Windows Azure Active Directory-Mandanten akzeptieren. Klicken Sie auf das folgende Bild, um eine erweiterte Ansicht der Änderungen an der Datei *Web. config* anzuzeigen.

     ![](windows-azure-authentication/_static/image9.png)
- Ein Dienst Prinzipal für Ihre Anwendung in Ihrem Windows Azure Active Directory-Mandanten wird bereitgestellt.
- HTTPS ist aktiviert.

## <a name="deploy-the-application-to-windows-azure"></a>Bereitstellen der Anwendung in Windows Azure

Umfassende Anweisungen finden Sie unter Bereitstellen [einer ASP.NET-Webanwendung auf einer Windows Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)-Website.

So veröffentlichen Sie eine Anwendung mithilfe der Windows Azure-Authentifizierung auf einer Azure-Website:

1. Klicken Sie mit der rechten Maustaste auf die Anwendung, und wählen Sie **veröffentlichen**

    ![](windows-azure-authentication/_static/image3.jpg)
2. Laden Sie im Dialogfeld "Web veröffentlichen" ein Veröffentlichungs Profil für Ihre Azure-Website herunter, und importieren Sie es.

    ![](windows-azure-authentication/_static/image4.jpg)
3. Auf der Registerkarte **Verbindung** wird die **Ziel-URL** angezeigt (die öffentliche URL für Ihre Anwendung). Klicken Sie zum Testen der Verbindung auf **Verbindung** überprüfen:

    ![](windows-azure-authentication/_static/image5.jpg)
4. Wenn Sie auf dieser Azure-Website veröffentlicht haben, sollten Sie die Einstellung **zusätzliche Dateien am Ziel entfernen** überprüfen, um sicherzustellen, dass Ihre Anwendung ordnungsgemäß veröffentlicht wird. Beachten Sie, dass das Kontrollkästchen **Windows Azure-Authentifizierung aktivieren aktiviert** ist.

    ![](windows-azure-authentication/_static/image10.png)
5. Optional: Klicken Sie auf der Registerkarte **Vorschau** auf **Vorschau starten** , um die bereitgestellten Dateien anzuzeigen.

    ![](windows-azure-authentication/_static/image6.jpg)
6. Klicken Sie auf **veröffentlichen.**

    Sie werden aufgefordert, die Windows Azure-Authentifizierung für den Zielhost zu aktivieren. Klicken Sie zum Fortfahren auf **aktivieren** :

    ![](windows-azure-authentication/_static/image11.png)
7. Geben Sie Ihre Administrator Anmelde Informationen für Ihren Windows Azure Active Directory-Mandanten ein:

    ![](windows-azure-authentication/_static/image7.jpg)
8. Nachdem Ihre Anwendung erfolgreich veröffentlicht wurde, wird ein Browser mit der veröffentlichten Website geöffnet.

    > [!NOTE]
    > Es kann bis zu fünf Minuten dauern (in der Regel muss weniger), damit Ihre Anwendung vollständig mit Windows-Azure Active Directory bereitgestellt wird, nachdem die Windows Azure-Authentifizierung für den Zielhost aktiviert wurde. Wenn Sie die Anwendung zum ersten Mal ausführen, wenn Sie Fehler ACS50001: die vertrauende Seite mit dem Namen "[Realm]" wurde nicht gefunden. warten Sie dann einige Minuten, und führen Sie die Anwendung erneut aus.
9. Melden Sie sich, wenn Sie dazu aufgefordert werden, als Benutzer in Ihrem Verzeichnis an:

    ![](windows-azure-authentication/_static/image8.jpg)
10. Sie haben sich nun mithilfe der Windows Azure-Authentifizierung erfolgreich bei ihrer in Azure gehosteten Anwendung angemeldet.

     ![](windows-azure-authentication/_static/image9.jpg)

## <a name="known-issues"></a>Bekannte Probleme

#### <a name="role-based-authorization-fails-when-using-windows-azure-authentication"></a>Die rollenbasierte Autorisierung schlägt bei Verwendung der Windows Azure-Authentifizierung fehl.

Von der Windows Azure-Authentifizierung wird derzeit nicht der erforderliche Rollen Anspruch bereitgestellt, damit die rollenbasierte Autorisierung ausgeführt werden kann. Die Rolle des authentifizierten Benutzers muss manuell von Windows Azure Active Directory abgerufen werden.

#### <a name="browsing-to-an-application-with-windows-azure-authentication-results-in-the-error-acs20016-the-domain-of-the-logged-in-user-livecom-does-not-match-any-allowed-domain-of-this-sts"></a>Das Navigieren zu einer Anwendung mit Windows Azure-Authentifizierung führt zu dem Fehler "ACS20016 die Domäne des angemeldeten Benutzers (Live.com) entspricht keiner zulässigen Domäne dieses STS".

Wenn Sie bereits bei einem Microsoft-Konto angemeldet sind (z. b. hotmail.com, Live.com, Outlook.com) und Sie versuchen, auf eine Anwendung zuzugreifen, für die die Windows Azure-Authentifizierung aktiviert ist, erhalten Sie möglicherweise eine 400-Fehler Antwort, weil die Domäne Ihres Microsoft-Kontos wird von Windows Azure Active Directory nicht erkannt. Melden Sie sich zunächst bei Ihrem Microsoft-Konto ab, um sich bei der Anwendung anzumelden.

#### <a name="logging-into-an-application-with-windows-azure-authentication-enabled-and-a-x509certificatevalidationmode-other-than-none-results-in-certificate-validation-errors-for-the-accountsaccesscontrolwindowsnet-certificate"></a>Das Anmelden bei einer Anwendung mit aktivierter Windows Azure-Authentifizierung und einer anderen X509CertificateValidationMode als "None" führt zu Zertifikat Validierungs Fehlern für das Accounts.AccessControl.Windows.net-Zertifikat.

Die Überprüfung des Zertifikats ist nicht erforderlich und sollte deaktiviert bleiben. Der Fingerabdruck des Aussteller Zertifikats wird von wsfederationauthenticationmodule überprüft.

#### <a name="when-attempting-to-enable-windows-azure-authentication-the-web-authentication-dialog-shows-the-error-acs20016-the-domain-of-the-logged-in-user-contosoonmicrosoftcom-does-not-match-any-allowed-domain-of-this-sts"></a>Wenn Sie versuchen, die Windows Azure-Authentifizierung zu aktivieren, wird im Dialogfeld "Webauthentifizierung" der folgende Fehler angezeigt: "ACS20016: die Domäne des angemeldeten Benutzers (contoso.onmicrosoft.com) entspricht keiner zulässigen Domäne dieses STS".

Dieser Fehler wird möglicherweise angezeigt, wenn Sie sich zuvor erfolgreich mit einem anderen Windows Azure Active Directory-Konto innerhalb desselben Visual Studio-Prozesses angemeldet haben. Melden Sie sich beim angegebenen Konto ab, oder starten Sie Visual Studio neu. Wenn Sie sich zuvor angemeldet und die Option "angemeldet bleiben" ausgewählt haben, müssen Sie möglicherweise Ihre Browser Cookies löschen.

## <a name="acs20012-the-request-is-not-a-valid-ws-federation-protocol-message"></a>ACS20012: die Anforderung ist keine gültige WS-Verbund-Protokollmeldung.

Dies kann vorkommen, wenn Sie bereits mit einer anderen Microsoft-ID für einen der Azure-Dienste angemeldet sind. Verwenden Sie das private Browserfenster wie "InPrivate" in IE oder "inkognito" in Chrome, oder löschen Sie alle Cookies.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Microsoft ASP.NET Tools für Windows Azure Active Directory – Visual Studio 2012](https://blogs.msdn.com/b/vbertocci/archive/2013/02/18/microsoft-asp-net-tools-for-windows-azure-active-directory-visual-studio-2012.aspx) – Vittorio berdecci
- [Windows Azure-Features: Identität](https://docs.microsoft.com/azure/active-directory/)
- [TechNet: Windows Azure Active Directory](https://technet.microsoft.com/library/hh967619.aspx)
- [Windows Azure Active Directory: Entwickeln von Apps für Ihre Organisation](https://activedirectory.windowsazure.com/Develop/Single-Tenant.aspx)
- [Windows Azure Active Directory: Entwickeln von Apps für mehrere Organisationen](https://activedirectory.windowsazure.com/Develop/Multi-Tenant.aspx)
- [So implementieren Sie Single Sign-on mit Windows Azure Active Directory](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect)
- [Einmaliges Anmelden mit Windows Azure Active Directory: Deep Dive](https://blogs.msdn.com/b/vbertocci/archive/2012/07/05/single-sign-on-with-windows-azure-active-directory-a-deep-dive.aspx) – Vittorio berwecci
- [Verwenden Sie AD FS 2,0 zum Implementieren und Verwalten von Single Sign-on](https://technet.microsoft.com/library/jj205462.aspx)
