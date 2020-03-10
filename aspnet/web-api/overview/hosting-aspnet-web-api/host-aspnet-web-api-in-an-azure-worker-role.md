---
uid: web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
title: Host ASP.net-Web-API 2 in einer Azure-workerrolle ASP.NET 4. x
author: MikeWasson
description: 'Tutorial: Hosten von ASP.net-Web-API in einer Azure-workerrolle mithilfe von owin, um das Web-API-Framework selbst zu hosten.'
ms.author: riande
ms.date: 04/02/2014
ms.custom: seoapril2019
ms.assetid: 6980ee2e-d6b0-4a08-8fb6-ab96362dd0e3
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: ec9904e0bff090be0f504036ae73977cfca0cb31
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78448407"
---
# <a name="host-aspnet-web-api-2-in-an-azure-worker-role"></a>Host ASP.net-Web-API 2 in einer Azure-workerrolle

von [Mike Wasson](https://github.com/MikeWasson)

> In diesem Tutorial wird gezeigt, wie Sie ASP.net-Web-API in einer Azure-workerrolle hosten, indem Sie owin zum Selbsthosten des Web-API-Frameworks
>
> [Open Web Interface for .net](http://owin.org/) (owin) definiert eine Abstraktion zwischen .net-Webservern und Webanwendungen. Owin entkoppelt die Webanwendung vom Server, wodurch owin ideal für eine Webanwendung in Ihrem eigenen Prozess, außerhalb von IIS, erstellt wird – z. b. innerhalb einer Azure-workerrolle.
>
> In diesem Tutorial verwenden Sie das Microsoft. owin. Host. HttpListener-Paket, das einen HTTP-Server bereitstellt, der zum selbst Hosten von owin-Anwendungen verwendet wird.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Im Tutorial verwendete Software Versionen
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - Web-API 2
> - [Azure SDK für .NET 2,3](https://azure.microsoft.com/downloads/)

## <a name="create-a-microsoft-azure-project"></a>Erstellen eines Microsoft Azure Projekts

Starten Sie Visual Studio mit Administratorrechten. Administrator Rechte sind erforderlich, um die Anwendung lokal mit dem Azure-Server Emulator zu debuggen.

Klicken Sie im Menü **Datei** auf **neu**, und klicken Sie dann auf **Projekt**. Klicken Sie in **installierte Vorlagen**unter C#Visual auf **Cloud** , und klicken Sie dann auf **Windows Azure-clouddienst**. Nennen Sie das Projekt "azureapp", und klicken Sie auf **OK**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image2.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image1.png)

Doppelklicken Sie im Dialogfeld **neuer Windows Azure-clouddienst** auf workerrolle. Belassen Sie den Standardnamen ("WorkerRole1"). In diesem Schritt wird der Projekt Mappe eine workerrolle hinzugefügt. Klicken Sie auf **OK**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image4.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image3.png)

Die erstellte Visual Studio-Projekt Mappe enthält zwei Projekte:

- &quot;azureapp&quot; definiert die Rollen und die Konfiguration für die Azure-Anwendung.
- &quot;WorkerRole1-&quot; enthält den Code für die workerrolle.

Im Allgemeinen kann eine Azure-Anwendung mehrere Rollen enthalten, obwohl in diesem Tutorial eine einzige Rolle verwendet wird.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-web-api-and-owin-packages"></a>Hinzufügen der Web-API und der owin-Pakete

Klicken Sie **im Menü** Extras auf **nuget-Paket-Manager**, und klicken Sie dann auf Paket-Manager- **Konsole**.

Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl ein:

[!code-console[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a>Hinzufügen eines HTTP-Endpunkts

Erweitern Sie in Projektmappen-Explorer das Projekt azureapp. Erweitern Sie den Knoten Rollen, klicken Sie mit der rechten Maustaste auf WorkerRole1, und wählen Sie **Eigenschaften**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image6.png)

Klicken Sie auf **Endpunkte**, und klicken Sie dann auf **Endpunkt hinzufügen**.

Wählen Sie in der Dropdown Liste **Protokoll** die Option "http" aus. Geben Sie unter **öffentlicher Port** und **privater Port**80 ein. Diese Portnummern dürfen sich unterscheiden. Der öffentliche Port wird vom Client verwendet, wenn er eine Anforderung an die Rolle sendet.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image8.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image7.png)

## <a name="configure-web-api-for-self-host"></a>Web-API für Self-Host konfigurieren

Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf das Projekt WorkerRole1, und wählen Sie / **Klasse** **Hinzufügen** , um eine neue Klasse hinzuzufügen Geben Sie der Klassen den Namen `Startup`.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image9.png)

Ersetzen Sie den gesamten Code in dieser Datei durch Folgendes:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample2.cs)]

## <a name="add-a-web-api-controller"></a>Hinzufügen eines Web-API-Controllers

Fügen Sie als nächstes eine Web-API-Controller Klasse hinzu. Klicken Sie mit der rechten Maustaste auf das Projekt WorkerRole1, und wählen Sie / **Klasse** **Hinzufügen** Benennen Sie die Klasse Testcontroller. Ersetzen Sie den gesamten Code in dieser Datei durch Folgendes:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample3.cs)]

Der Einfachheit halber definiert dieser Controller nur zwei Get-Methoden, die nur Text zurückgeben.

## <a name="start-the-owin-host"></a>Starten des owin-Hosts

Öffnen Sie die Datei WorkerRole.cs. Diese Klasse definiert den Code, der ausgeführt wird, wenn die workerrolle gestartet und beendet wird.

Fügen Sie die folgende using-Anweisung hinzu:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample4.cs)]

Fügen Sie der `WorkerRole`-Klasse ein **iverwerfbares** Element hinzu:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample5.cs)]

Fügen Sie in der `OnStart`-Methode den folgenden Code hinzu, um den Host zu starten:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample6.cs?highlight=5)]

Die **webapp. Start** -Methode startet den owin-Host. Der Name der `Startup` Klasse ist ein Typparameter für die Methode. Gemäß der Konvention ruft der Host die `Configure`-Methode dieser Klasse auf.

Überschreiben Sie die `OnStop`, um die *\_App* -Instanz zu löschen:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample7.cs)]

Im folgenden finden Sie den gesamten Code für WorkerRole.cs:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample8.cs)]

Erstellen Sie die Projekt Mappe, und drücken Sie F5, um die Anwendung lokal im Azure-Server Emulator auszuführen. Abhängig von den Firewalleinstellungen müssen Sie den Emulator möglicherweise über die Firewall zulassen.

> [!NOTE]
> Wenn Sie eine Ausnahme wie die folgende erhalten, finden Sie in [diesem Blogbeitrag](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) eine Problem Umgehung. "Die Datei oder Assembly ' Microsoft. owin, Version = 2.0.2.0, Culture = neutral, PublicKeyToken = 31bs3856ad364e35 ' oder eine ihrer Abhängigkeiten konnte nicht geladen werden. Die Manifest-Definition der lokalisierte Assembly stimmt nicht mit dem Assemblyverweis. (Ausnahme von HRESULT: 0x80131040) "

Der Server Emulator weist dem Endpunkt eine lokale IP-Adresse zu. Sie finden die IP-Adresse, indem Sie die Server Emulator-Benutzeroberfläche anzeigen. Klicken Sie im Benachrichtigungsbereich der Taskleiste mit der rechten Maustaste auf das Symbol Emulator, und wählen Sie Server **Emulator-Benutzeroberfläche anzeigen**aus.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image11.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image10.png)

Suchen Sie die IP-Adresse unter Dienst Bereitstellungen, Bereitstellung [ID] und Dienst Details. Öffnen Sie einen Webbrowser, und navigieren Sie zu http://<em>Address</em>/Test/1, wobei <em>Address</em> die IP-Adresse ist, die vom Server Emulator zugewiesen wird. beispielsweise `http://127.0.0.1:80/test/1`. Die Antwort des Web-API-Controllers sollte angezeigt werden:

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image12.png)

## <a name="deploy-to-azure"></a>Bereitstellung in Azure

Für diesen Schritt müssen Sie über ein Azure-Konto verfügen. Wenn Sie noch nicht über eins verfügen, können Sie in wenigen Minuten ein kostenloses Testkonto erstellen. Weitere Informationen finden Sie unter [Microsoft Azure kostenlosen Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).

Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf das Projekt azureapp. Wählen Sie **Veröffentlichen**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image13.png)

Wenn Sie nicht bei Ihrem Azure-Konto angemeldet sind, klicken Sie auf **Anmelden**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image15.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image14.png)

Nachdem Sie angemeldet sind, wählen Sie ein Abonnement aus, und klicken Sie auf **weiter**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image17.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image16.png)

Geben Sie einen Namen für den clouddienst ein, und wählen Sie eine Region aus. Klicken Sie auf **Erstellen**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image18.png)

Klicken Sie auf **Veröffentlichen**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image20.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image19.png)

Das Fenster Azure-Aktivitätsprotokoll zeigt den Fortschritt der Bereitstellung an. Wenn die APP bereitgestellt ist, navigieren Sie zu http://appname.cloudapp.net/test/1.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image21.png)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Übersicht über das Katana-Projekt](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)
- [Katana-Projekt auf GitHub](https://github.com/aspnet/AspNetKatana)
