---
uid: web-api/overview/older-versions/self-host-a-web-api
title: Self-Host ASP.net-Web-API 1 (C#)-ASP.NET 4. x
author: MikeWasson
description: Tutorial mit Code zeigt, wie eine Web-API in einer Konsolenanwendung gehostet wird.
ms.author: riande
ms.date: 01/26/2012
ms.custom: seoapril2019
ms.assetid: be5ab1e2-4140-4275-ac59-ca82a1bac0c1
msc.legacyurl: /web-api/overview/older-versions/self-host-a-web-api
msc.type: authoredcontent
ms.openlocfilehash: bae1737ba5b16bc67fa0ed0474ff04df0add1b3a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78421371"
---
# <a name="self-host-aspnet-web-api-1-c"></a>Self-Host-ASP.net-Web-API 1C#()

von [Mike Wasson](https://github.com/MikeWasson)

> In diesem Tutorial wird gezeigt, wie Sie eine Web-API in einer Konsolenanwendung hosten. ASP.net-Web-API ist IIS nicht erforderlich. Sie können eine Web-API in Ihrem eigenen Host Prozess selbst hosten. 
> 
> **Neue Anwendungen sollten owin für die Self-Host-Web-API verwenden.** Weitere Informationen finden [Sie unter Verwenden von owin für Self-Host ASP.net-Web-API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Im Tutorial verwendete Software Versionen
> 
> 
> - Web-API 1
> - Visual Studio 2012

## <a name="create-the-console-application-project"></a>Erstellen des Konsolen Anwendungs Projekts

Starten Sie Visual Studio, und wählen Sie auf der **Start** Seite die Option **Neues Projekt** aus. Oder wählen Sie im Menü **Datei** die Option **neu** und dann **Projekt**aus.

Wählen Sie im Bereich **Vorlagen** die Option **installierte Vorlagen** aus, und erweitern Sie den Knoten **visuelle C#**  Knoten. Wählen Sie unter **Visualisierung C#** die Option **Windows**aus. Wählen Sie in der Liste der Projektvorlagen die Option **Konsolenanwendung**aus. Nennen Sie das Projekt &quot;SelfHost-&quot;, und klicken Sie auf **OK**.

![](self-host-a-web-api/_static/image1.png)

## <a name="set-the-target-framework-visual-studio-2010"></a>Festlegen des Ziel Frameworks (Visual Studio 2010)

Wenn Sie Visual Studio 2010 verwenden, ändern Sie das Ziel Framework in .NET Framework 4,0. (Standardmäßig ist die Projektvorlage auf das [.NET Framework-Client Profil](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile)ausgerichtet.)

Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und wählen Sie **Eigenschaften**aus. Ändern Sie in der Dropdown Liste **Ziel Framework** das Ziel Framework in .NET Framework 4,0. Wenn Sie aufgefordert werden, die Änderung anzuwenden, klicken Sie auf **Ja**.

![](self-host-a-web-api/_static/image2.png)

## <a name="install-nuget-package-manager"></a>Installieren des nuget-Paket-Managers

Der nuget-Paket-Manager ist die einfachste Möglichkeit, die Web-API-Assemblys zu einem Non-ASP.net-Projekt hinzuzufügen.

Um zu prüfen, **ob der** nuget-Paket-Manager installiert ist, klicken Sie in Visual Studio auf das Menü Extras. Wenn Sie ein Menü Element namens **nuget-Paket-Manager**sehen, haben Sie den nuget-Paket-Manager.

So installieren Sie den nuget-Paket-Manager:

1. Starten Sie Visual Studio.
2. Wählen Sie im Menü **Extras** die Option **Erweiterungen und Updates** aus.
3. Wählen Sie im Dialogfeld **Erweiterungen und Updates** die Option **Online**aus.
4. Wenn "nuget-Paket-Manager" nicht angezeigt wird, geben Sie "nuget-Paket-Manager" in das Suchfeld ein.
5. Wählen Sie den nuget-Paket-Manager aus, **und klicken Sie**auf
6. Nachdem der Download abgeschlossen ist, werden Sie aufgefordert, zu installieren.
7. Nachdem die Installation abgeschlossen ist, werden Sie möglicherweise aufgefordert, Visual Studio neu zu starten.

![](self-host-a-web-api/_static/image3.png)

## <a name="add-the-web-api-nuget-package"></a>Hinzufügen des Web-API-nuget-Pakets

Fügen Sie nach der Installation des nuget-Paket-Managers das Web-API-Self-Host-Paket zu Ihrem Projekt hinzu.

1. Wählen Sie **im Menü** Extras den Befehl **nuget-Paket-Manager**aus. *Hinweis*: Wenn dieses Menü Element nicht angezeigt wird, stellen Sie sicher, dass der nuget-Paket-Manager ordnungsgemäß installiert ist.
2. Wählen Sie **nuget-Pakete für Projekt Mappe verwalten aus** .
3. Klicken Sie im Dialogfeld " **nuget-Pakete verwalten** " auf **Online**.
4. Geben Sie im Suchfeld &quot;Microsoft. Aspnet. WebAPI. SelfHost&quot;ein.
5. Wählen Sie das ASP.net-Web-API selbsthostpaket aus, und klicken Sie auf **Installieren**.
6. Nachdem das Paket installiert wurde, klicken Sie auf **Schließen** , um das Dialogfeld zu schließen.

> [!NOTE]
> Stellen Sie sicher, dass Sie das Paket mit dem Namen "Microsoft. Aspnet. WebAPI. SelfHost" installieren, nicht "aspnetwebapi. SelfHost".

![](self-host-a-web-api/_static/image4.png)

## <a name="create-the-model-and-controller"></a>Erstellen des Modells und des Controllers

In diesem Tutorial werden die gleichen Modell-und Controller Klassen wie im Tutorial " [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) " verwendet.

Fügen Sie eine öffentliche Klasse mit dem Namen `Product`hinzu.

[!code-csharp[Main](self-host-a-web-api/samples/sample1.cs)]

Fügen Sie eine öffentliche Klasse mit dem Namen `ProductsController`hinzu. Leiten Sie diese Klasse von **System. Web. http. apicontroller**ab.

[!code-csharp[Main](self-host-a-web-api/samples/sample2.cs)]

Weitere Informationen zum Code in diesem Controller finden Sie im Tutorial [zu](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) den ersten Schritten. Dieser Controller definiert drei Get-Aktionen:

| URI | Beschreibung |
| --- | --- |
| /api/products | Eine Liste aller Produkte erhalten. |
| /API/Products/-*ID* | Erhalten Sie ein Produkt nach ID. |
| /API/Products/? Category =*Kategorie* | Eine Liste der Produkte nach Kategorie erhalten. |

## <a name="host-the-web-api"></a>Hosten der Web-API

Öffnen Sie die Datei Program.cs, und fügen Sie die folgenden using-Anweisungen hinzu:

[!code-csharp[Main](self-host-a-web-api/samples/sample3.cs)]

Fügen Sie der **Program** -Klasse den folgenden Code hinzu.

[!code-csharp[Main](self-host-a-web-api/samples/sample4.cs)]

## <a name="optional-add-an-http-url-namespace-reservation"></a>Optionale Hinzufügen einer HTTP-URL-Namespace Reservierung

Diese Anwendung lauscht auf `http://localhost:8080/`. Standardmäßig sind für das lauschen an einer bestimmten http-Adresse Administratorrechte erforderlich. Wenn Sie das Tutorial ausführen, erhalten Sie möglicherweise den folgenden Fehler: "http konnte die URL http://+:8080/nicht registrieren". es gibt zwei Möglichkeiten, diesen Fehler zu vermeiden:

- Führen Sie Visual Studio mit erweiterten Administrator Berechtigungen aus, oder
- Verwenden Sie netsh. exe, um Ihrem Konto Berechtigungen zum Reservieren der URL zu übergeben.

Um "Netsh. exe" zu verwenden, öffnen Sie eine Eingabeaufforderung mit Administratorrechten, und geben Sie den folgenden Befehl ein:

[!code-console[Main](self-host-a-web-api/samples/sample5.cmd)]

Dabei ist " *" Computer\Benutzername "* " Ihr Benutzerkonto.

Wenn Sie das selbst Hosting abgeschlossen haben, stellen Sie sicher, dass Sie die Reservierung löschen:

[!code-console[Main](self-host-a-web-api/samples/sample6.cmd)]

## <a name="call-the-web-api-from-a-client-application-c"></a>Abrufen der Web-API aus einer Client AnwendungC#()

Wir schreiben eine einfache Konsolenanwendung, die die Web-API aufruft.

Fügen Sie der Projekt Mappe ein neues Konsolen Anwendungsprojekt hinzu:

- Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf die Projekt Mappe, und wählen Sie **Neues Projekt hinzufügen**
- Erstellen Sie eine neue Konsolenanwendung mit dem Namen &quot;ClientApp&quot;.

![](self-host-a-web-api/_static/image5.png)

Fügen Sie mit dem nuget-Paket-Manager das ASP.net-Web-API Core-Bibliothekspaket hinzu:

- Wählen Sie im Menü Extras den Befehl **nuget-Paket-Manager**aus.
- Wählen Sie **nuget-Pakete für Projekt Mappe verwalten aus** .
- Klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf **Online**.
- Geben Sie im Suchfeld &quot;Microsoft. Aspnet. WebAPI. Client&quot;ein.
- Wählen Sie das Paket Microsoft ASP.net Web-API Client Libraries aus, und klicken Sie auf **Installieren**.

Fügen Sie dem Projekt "SelfHost" in "ClientApp" einen Verweis hinzu:

- Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf das ClientApp-Projekt.
- Klicken Sie auf **Verweis hinzufügen**.
- Wählen **Sie im Dialog**Feld **Verweis-Manager** Unterprojekt Mappe die Option **Projekte**aus.
- Wählen Sie das Projekt SelfHost aus.
- Klicken Sie auf **OK**.

![](self-host-a-web-api/_static/image6.png)

Öffnen Sie die Datei "Client/Program. cs". Fügen Sie die folgende **using** -Anweisung hinzu:

[!code-csharp[Main](self-host-a-web-api/samples/sample7.cs)]

Fügen Sie eine statische **HttpClient** -Instanz hinzu:

[!code-csharp[Main](self-host-a-web-api/samples/sample8.cs)]

Fügen Sie die folgenden Methoden hinzu, um alle Produkte aufzulisten, ein Produkt nach ID aufzulisten und Produkte nach Kategorie aufzulisten.

[!code-csharp[Main](self-host-a-web-api/samples/sample9.cs)]

Jede dieser Methoden folgt dem gleichen Muster:

1. Aufrufen von **HttpClient. getasync** , um eine GET-Anforderung an den entsprechenden URI zu senden.
2. Rufen Sie **HttpResponseMessage. ensuresuccess Statuscode**auf. Diese Methode löst eine Ausnahme aus, wenn der HTTP-Antwortstatus ein Fehlercode ist.
3. Aufrufen von "read **asasync&lt;t&gt;** ", um einen CLR-Typ aus der HTTP-Antwort zu deserialisieren. Diese Methode ist eine Erweiterungsmethode, die in **System .net. http. httpcontentextensions**definiert ist.

Die Methoden **getasync** und Read **asasync** sind beide asynchron. Sie geben **Aufgaben** Objekte zurück, die den asynchronen Vorgang darstellen. Durch Abrufen der **Ergebnis** Eigenschaft wird der Thread blockiert, bis der Vorgang abgeschlossen ist.

Weitere Informationen zur Verwendung von HttpClient, einschließlich der Vorgehensweise zum Erstellen von nicht blockierenden aufrufen, finden Sie unter [Aufrufen einer Web-API von einem .NET-Client](../advanced/calling-a-web-api-from-a-net-client.md).

Bevor Sie diese Methoden aufrufen, legen Sie die BaseAddress-Eigenschaft der httpclient-Instanz auf "`http://localhost:8080`" fest. Beispiel:

[!code-csharp[Main](self-host-a-web-api/samples/sample10.cs)]

Dadurch sollte Folgendes ausgegeben werden. (Denken Sie daran, die SelfHost-Anwendung zuerst auszuführen.)

[!code-console[Main](self-host-a-web-api/samples/sample11.cmd)]

![](self-host-a-web-api/_static/image7.png)
