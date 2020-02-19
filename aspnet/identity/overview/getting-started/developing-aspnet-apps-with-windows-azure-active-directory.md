---
uid: identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory
title: Entwickeln von ASP.net-apps mit Azure Active Directory-ASP.NET 4. x
author: Rick-Anderson
description: Mit Microsoft ASP.NET Tools für Azure Active Directory können Sie die Authentifizierung für in Azure gehostete Webanwendungen einfach aktivieren. Sie können Azure au verwenden...
ms.author: riande
ms.date: 08/14/2014
ms.assetid: 457d7eaf-ee76-4ceb-9082-c7c1721435ad
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory
msc.type: authoredcontent
ms.openlocfilehash: 28425ea8d1312dfc6e14df9677396f2cbcf6f16d
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2020
ms.locfileid: "77456724"
---
# <a name="developing-aspnet-apps-with-azure-active-directory"></a>Entwickeln von ASP.NET-Apps mit Azure Active Directory

von [Rick Anderson](https://twitter.com/RickAndMSFT)

Microsoft ASP.NET Tools für Azure Active Directory vereinfachen das Aktivieren der Authentifizierung für in [Azure](https://www.windowsazure.com/home/features/web-sites/)gehostete Web-Apps. Sie können die Azure-Authentifizierung verwenden, um Office 365-Benutzer von Ihrer Organisation aus zu authentifizieren, Unternehmenskonten, die von Ihrer lokalen Active Directory synchronisiert werden, oder Benutzer, die in ihrer eigenen benutzerdefinierten Azure Active Directory Domäne erstellt wurden. Durch Aktivieren der Windows Azure-Authentifizierung wird Ihre Anwendung für die Authentifizierung von Benutzern mithilfe eines einzelnen [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) Mandanten konfiguriert.

In diesem Tutorial wird gezeigt, wie Sie eine ASP.NET-Anwendung erstellen, die für die Anmeldung mit [Azure Active Directory](https://msdn.microsoft.com/library/azure/mt168838.aspx) (Azure AD) konfiguriert ist. Außerdem erfahren Sie, wie Sie den Graph-API zum Abrufen von Informationen über den aktuell angemeldeten Benutzer abrufen und wie Sie die Anwendung in Azure bereitstellen.

## <a name="prerequisites"></a>Voraussetzungen

1. [Visual Studio Express 2013 für Web](https://my.visualstudio.com/Downloads?q=visual%20studio%202013#d-2013-express) oder [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013).
2. [Visual Studio 2013 Update 4](https://www.microsoft.com/download/details.aspx?id=44921) -Update 3 oder höher ist erforderlich.
3. Ein Azure-Konto. [Klicken Sie hier](https://azure.microsoft.com/pricing/free-trial/) , um eine kostenlose Testversion zu erhalten, wenn Sie nicht bereits über ein Konto verfügen.

## <a name="add-a-global-administrator-to-your-active-directory"></a>Fügen Sie Ihrem Active Directory einen globalen Administrator hinzu.

1. Melden Sie sich beim [Azure-Verwaltungsportal](https://manage.windowsazure.com/)an.
2. Alle Azure-Konten enthalten ein **Standardverzeichnis** . Klicken Sie darauf, und klicken Sie dann oben auf der Seite auf die Registerkarte **Benutzer** (siehe Abbildung unten).
3. Klicken Sie auf „Benutzer hinzufügen“.
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image1.png)
4. Erstellen Sie einen neuen Benutzer mit der Rolle **globaler Administrator** . Klicken Sie im oberen Menü auf **Benutzer** , und klicken Sie dann auf der Befehlsleiste auf die Schaltfläche **Benutzer hinzufügen** .
5. Geben Sie im Dialogfeld **Benutzer hinzufügen** einen Namen für den neuen Benutzer ein, und klicken Sie dann auf den Pfeil nach rechts.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image2.png)
6. Geben Sie den Benutzernamen ein, und legen Sie die Rolle auf **globaler Administrator**fest. Globale Administratoren benötigen eine Alternative e-Mail-Adresse für die Kenn Wort Wiederherstellung. Wenn Sie fertig sind, klicken Sie auf den Pfeil nach rechts.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image3.png)
7. Klicken Sie auf der nächsten Seite des Dialog Felds auf **Erstellen**. Für den neuen Benutzer wird ein temporäres Kennwort erstellt, das im Dialogfeld angezeigt wird.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image4.png)

   Speichern Sie das Kennwort. Sie müssen das Kennwort nach der ersten Anmeldung ändern. In der folgenden Abbildung wird das neue Administrator Konto angezeigt. Sie müssen den Azure Active Directory verwenden, um sich bei ihrer App anzumelden, nicht die Microsoft-Konto, die auch auf dieser Seite angezeigt wird.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image5.png)

## <a name="create-an-aspnet-application"></a>Erstellen einer ASP.NET-Anwendung

In den folgenden Schritten wird [Visual Studio Express 2013 für das Web](https://www.microsoft.com/download/details.aspx?id=40747)verwendet, und es ist [Visual Studio 2013 Update 3](https://www.microsoft.com/download/details.aspx?id=43721)erforderlich.

1. Klicken Sie in Visual Studio auf **Datei** und dann auf **Neues Projekt**. Wählen Sie im Dialogfeld **Neues Projekt** im Menü C# auf der linken Seite das visuelle Webprojekt aus, und klicken Sie auf **OK**. Möglicherweise möchten Sie auch die Option **Application Insights zu Projekt hinzufügen** deaktivieren, wenn Sie die Funktionalität Ihrer Anwendung nicht möchten.
2. Wählen Sie im Dialogfeld **Neues ASP.net-Projekt** die Option **MVC**aus, und klicken Sie dann auf **Authentifizierung ändern**.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image6.png)
3. Wählen Sie im Dialogfeld **Authentifizierung ändern** die Option **Organisations Konten**aus. Diese Optionen können verwendet werden, um Ihre Anwendung automatisch bei Azure AD zu registrieren und die Anwendung automatisch für die Integration in Azure AD zu konfigurieren. Sie müssen das Dialogfeld " **Authentifizierung ändern** " nicht verwenden, um Ihre Anwendung zu registrieren und zu konfigurieren, aber es ist viel einfacher. Wenn Sie z. b. Visual Studio 2012 verwenden, können Sie die Anwendung weiterhin manuell im Azure-Verwaltungsportal registrieren und die Konfiguration für die Integration in Azure AD aktualisieren.
   Wählen Sie in den Dropdown Menüs die Option **Cloud-Single Organization** und **einmaliges Anmelden, Verzeichnis Daten lesen**aus. Geben Sie die Domäne für Ihr Azure AD Verzeichnis ein, z. b. (in den folgenden Images) *aricka0yahoo.onmicrosoft.com*, und klicken Sie dann auf **OK**. Sie können den Domänen Namen auf der Registerkarte Domänen für das Standardverzeichnis im Azure-Portal abrufen (siehe die nächste Abbildung unten).

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image7.png)

   Die folgende Abbildung zeigt den Domänen Namen aus der Azure-Portal.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image8.png)

    > [!NOTE]
    > Sie können optional den Anwendungs-ID-URI konfigurieren, der in Azure AD registriert wird, indem Sie auf **Weitere Optionen**klicken. Der APP-ID-URI ist der eindeutige Bezeichner für eine Anwendung, die in Azure AD registriert ist und von der Anwendung verwendet wird, um sich bei der Kommunikation mit Azure AD zu identifizieren. Weitere Informationen zum APP-ID-URI und anderen Eigenschaften registrierter Anwendungen finden Sie in [diesem Thema](https://msdn.microsoft.com/library/azure/dn499820.aspx#BKMK_Registering). Wenn Sie auf das Kontrollkästchen unterhalb des Felds APP-ID-URI klicken, können Sie auch eine vorhandene Registrierung in Azure AD überschreiben, die denselben APP-ID-URI verwendet.
4. Nachdem Sie auf " **OK**" geklickt haben, wird ein Anmelde Dialogfeld angezeigt, und Sie müssen sich mit einem globalen Administrator Konto (nicht mit dem Microsoft-Konto, das Ihrem Abonnement zugeordnet ist) anmelden. Wenn Sie zuvor ein neues Administrator Konto erstellt haben, müssen Sie das Kennwort ändern und sich dann mit dem neuen Kennwort erneut anmelden.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image9.png)
5. Nachdem Sie sich erfolgreich authentifiziert haben, werden im Dialogfeld " **Neues ASP.net-Projekt** " Ihre Authentifizierungs Auswahl (**Organisation** ) und das Verzeichnis angezeigt, in dem die neue Anwendung registriert wird (*aricka0yahoo.onmicrosoft.com* in der folgenden Abbildung). Wählen Sie unter diese Informationen das Kontrollkästchen **Host in der Cloud aus**. Wenn dieses Kontrollkästchen aktiviert ist, wird das Projekt als Azure-Web-App bereitgestellt und wird später für eine einfache Veröffentlichung aktiviert. Klicken Sie auf **OK**.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image10.png)
6. Das Dialogfeld **Azure-Website konfigurieren** wird unter Verwendung eines automatisch generierten Website namens und einer Region angezeigt. Beachten Sie auch das Konto, bei dem Sie zurzeit im Dialogfeld angemeldet sind. Sie möchten sicherstellen, dass dieses Konto das Konto ist, mit dem Ihr Azure-Abonnement verbunden ist, in der Regel eine Microsoft-Konto.

    > [!NOTE]
    > Für dieses Projekt ist eine Datenbank erforderlich. Sie müssen eine Ihrer vorhandenen Datenbanken auswählen oder eine neue erstellen. Eine Datenbank ist erforderlich, da das Projekt bereits eine lokale Datenbankdatei verwendet, um eine kleine Menge von Authentifizierungs Konfigurationsdaten zu speichern. Wenn Sie die Anwendung auf einer Azure-Website bereitstellen, wird diese Datenbank nicht mit der Bereitstellung verpackt, daher müssen Sie eine auswählen, auf die in der Cloud zugegriffen werden kann. Klicken Sie auf **OK**.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image11.png)
7. Das Projekt wird erstellt, und Ihre Authentifizierungs Optionen und Web-App-Optionen werden automatisch mit dem Projekt konfiguriert. Nachdem dieser Vorgang abgeschlossen wurde, führen Sie das Projekt lokal aus, indem Sie **^ F5**drücken. Sie müssen sich mit Ihrem Organisations Konto anmelden. Geben Sie den Benutzernamen und das Kennwort für das zuvor erstellte Konto ein, und klicken Sie auf **Anmelden**.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image12.png)
8. Nach erfolgreicher Anmeldung wird auf der ASP.NET-Website angezeigt, dass Sie sich authentifiziert haben, indem Sie den Benutzernamen in der oberen rechten Ecke der Seite anzeigen.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image13.png)

   Wenn Sie den Fehler erhalten: der Wert darf nicht NULL oder leer sein. Parameter Name: Linktext ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image14.png)

   Weitere Informationen finden Sie im Abschnitt zum [Debuggen](#dbg) am Ende des Tutorials.

## <a name="basics-of-the-graph-api"></a>Grundlagen der Graph-API

[Der Graph-API](https://msdn.microsoft.com/library/azure/hh974476.aspx) ist die programmgesteuerte Schnittstelle, mit der CRUD und andere Vorgänge für Objekte in Ihrem Azure AD Verzeichnis durchgeführt werden. Wenn Sie beim Erstellen eines neuen Projekts in Visual Studio 2013 eine Option für das Organisations Konto für die Authentifizierung auswählen, wird die Anwendung bereits so konfiguriert, dass Sie die Graph-API aufruft. In diesem Abschnitt wird kurz erläutert, wie die Graph-API funktioniert.

1. Klicken Sie in der laufenden Anwendung oben rechts auf der Seite auf den Namen des angemeldeten Benutzers. Dadurch gelangen Sie zur Seite "Benutzerprofil", bei der es sich um eine Aktion auf dem Home-Controller handelt. Sie werden feststellen, dass die Tabelle Benutzerinformationen zum Administrator Konto enthält, das Sie zuvor erstellt haben. Diese Informationen werden in Ihrem Verzeichnis gespeichert, und der Graph-API wird aufgerufen, um diese Informationen abzurufen, wenn die Seite geladen wird.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image15.png)
2. Wechseln Sie zurück zu Visual Studio, und erweitern Sie den Ordner **Controllers** , und öffnen Sie dann die Datei **HomeController.cs** . Sie sehen eine **UserProfile ()** -Aktion, die Code zum Abrufen eines Tokens enthält, und dann den Graph-API aufrufen. Dieser Code wird im folgenden dupliziert:

    [!code-csharp[Main](developing-aspnet-apps-with-windows-azure-active-directory/samples/sample1.cs?highlight=22)]

    Um die Graph-API aufzurufen, müssen Sie zunächst ein Token abrufen. Wenn das Token abgerufen wird, muss der zugehörige Zeichen folgen Wert im Autorisierungs Header für alle nachfolgenden Anforderungen an die Graph-API angehängt werden. Der größte Teil des obigen Codes behandelt die Details der Authentifizierung bei Azure AD zum Abrufen eines Tokens, zum Abrufen des Graph-API mithilfe des Tokens und zum anschließenden Transformieren der Antwort, sodass Sie in der Ansicht angezeigt werden kann.

    Der relevanteste Teil zur Erörterung ist die folgende hervorgehobene Zeile: `UserProfile profile = JsonConvert.DeserializeObject<UserProfile>(responseString);`. Diese Zeile steht für den Namen des Benutzers, der aus der JSON-Antwort deserialisiert wurde und in der Ansicht angezeigt wird.

    Sie können den Graph-API mithilfe von HttpClient aufrufen und die Rohdaten selbst verarbeiten, aber eine einfachere Möglichkeit ist die Verwendung der [Graph-Client Bibliothek, die über nuget verfügbar ist](http://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient/). Die Client Bibliothek verarbeitet die unformatierten HTTP-Anforderungen und die Transformation der zurückgegebenen Daten für Sie und erleichtert die Arbeit mit dem Graph-API in einer .NET-Umgebung. Weitere Informationen finden Sie in den zugehörigen Graph-API-Codebeispielen auf [GitHub.](https://github.com/AzureADSamples)

## <a name="deploy-the-application-to-azure"></a>Bereitstellen der Anwendung in Azure

In den folgenden Schritten wird gezeigt, wie Sie die Anwendung in Azure bereitstellen. In den vorherigen Schritten haben Sie das neue Projekt mit einer Web-App in Azure verbunden, sodass es in nur wenigen Schritten veröffentlicht werden kann.

1. Klicken Sie in Visual Studio mit der rechten Maustaste auf das Projekt, und wählen Sie **veröffentlichen**aus. Das Dialogfeld **Web veröffentlichen** wird angezeigt, wobei jede Einstellung bereits konfiguriert ist. Klicken Sie auf die Schaltfläche **weiter** , um zur Seite **Einstellungen** zu gelangen. Sie werden möglicherweise aufgefordert, sich zu authentifizieren. Stellen Sie sicher, dass Sie sich mit Ihrem Azure-Abonnement Konto (in der Regel eine Microsoft-Konto) und nicht mit dem zuvor erstellten Organisations Konto authentifizieren.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image16.png)
2. Aktivieren Sie die Option **Organisations Authentifizierung aktivieren** . Geben Sie im Feld **Domäne** die Domäne für Ihr Verzeichnis ein. Wählen Sie in der Dropdown-Dropdown-Dropdown-Option **einmaliges Anmelden, Verzeichnis Daten lesen**aus. Sie werden feststellen, dass die vorherige Datenbank, die Sie verwendet haben, bereits im Abschnitt **Datenbanken** aufgefüllt ist. Klicken Sie auf **Veröffentlichen**.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image17.png)
3. Visual Studio beginnt mit der Bereitstellung Ihrer Website, und ein neues Browserfenster wird angezeigt. Sie werden möglicherweise erneut aufgefordert, sich erneut bei Ihrem Verzeichnis zu authentifizieren. Nachdem Sie sich authentifiziert haben, werden Sie auf die neu veröffentlichte Website in Azure umgeleitet.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image18.png)

<a id="dbg"></a>
## <a name="debugging-the-app"></a>Debuggen der APP

Wenn Sie den folgenden Fehler erhalten: der Wert darf nicht NULL oder leer sein. Parameter Name: Linktext

![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image19.png)

Ersetzen Sie den Code in der Datei *views\shared\\_LoginPartial. cshtml* durch Folgendes:

[!code-cshtml[Main](developing-aspnet-apps-with-windows-azure-active-directory/samples/sample2.cshtml?highlight=1-8,15-16)]

Wenn der angemeldete Benutzer nach dem Ausführen der APP den Wert "Null User" anzeigt, melden Sie sich ab, und melden Sie sich mit dem zuvor erstellten Active Directory Konto wieder an.

Im folgenden finden Sie ein hervorragendes Tutorial, das Rick Rainey ausführlich erläutert [: Azure Websites und die Organisations Authentifizierung mit Azure AD](http://rickrainey.com/2014/08/19/deep-dive-azure-websites-and-organizational-authentication-using-azure-ad/).

## <a name="more-information"></a>Weitere Informationen

- [Deep Dive: Azure Websites und die Organisations Authentifizierung mit Azure AD](http://rickrainey.com/2014/08/19/deep-dive-azure-websites-and-organizational-authentication-using-azure-ad/)
- [Übersicht über Azure AD Graph-API](https://msdn.microsoft.com/library/azure/hh974476.aspx)
- [Authentifizierungs Szenarien in Azure AD](https://msdn.microsoft.com/library/azure/dn499820.aspx)
- [Azure Ad Code Beispiele auf GitHub](https://github.com/AzureADSamples)
