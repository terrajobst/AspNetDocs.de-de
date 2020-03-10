---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
title: Erstellen einer odata V4-Client-C#app () | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/26/2014
ms.assetid: 47202362-3808-4add-9a69-c9d1f91d5e4e
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
msc.type: authoredcontent
ms.openlocfilehash: a0016cf2cc7bffe6268664395ccb38e140090310
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78448119"
---
# <a name="create-an-odata-v4-client-app-c"></a>Erstellen einer OData v4-Client-App (C#)

von [Mike Wasson](https://github.com/MikeWasson)

Im vorherigen Tutorial haben Sie einen grundlegenden odata-Dienst erstellt, der CRUD-Vorgänge unterstützt. Nun erstellen wir einen Client für den Dienst.

Starten Sie eine neue Instanz von Visual Studio, und erstellen Sie ein neues Konsolen Anwendungsprojekt. Wählen Sie im Dialogfeld **Neues Projekt** die Option **installierte** &gt; **Vorlagen** &gt;  **C# Visual** &gt; **Windows-Desktop**aus, und wählen Sie die Vorlage **Konsolenanwendung** aus. Nennen Sie das Projekt &quot;productapp&quot;.

![](create-an-odata-v4-client-app/_static/image1.png)

> [!NOTE]
> Sie können auch die Konsolen-App zur gleichen Visual Studio-Projekt Mappe hinzufügen, die den odata-Dienst enthält.

## <a name="install-the-odata-client-code-generator"></a>Installieren des odata-Client Code-Generators

Wählen Sie im Menü **Extras** die Option **Erweiterungen und Updates** aus. Wählen Sie **Online** &gt; **Visual Studio Gallery**aus. Suchen Sie im Suchfeld nach &quot;odata-Client Code-Generator&quot;. Klicken Sie zum Installieren der VSIX auf **herunterladen** . Möglicherweise werden Sie aufgefordert, Visual Studio neu zu starten.

[![](create-an-odata-v4-client-app/_static/image3.png)](create-an-odata-v4-client-app/_static/image2.png)

## <a name="run-the-odata-service-locally"></a>Lokales Ausführen des odata-Diensts

Führen Sie das productservice-Projekt in Visual Studio aus. Standardmäßig wird von Visual Studio ein Browser zum Stammverzeichnis der Anwendung gestartet. Beachten Sie den URI. Dies wird im nächsten Schritt benötigt. Unterbrechen Sie die Ausführung der Anwendung nicht.

![](create-an-odata-v4-client-app/_static/image4.png)

> [!NOTE]
> Wenn Sie beide Projekte in dieselbe Projekt Mappe einfügen, stellen Sie sicher, dass das productservice-Projekt ohne Debuggen ausgeführt wird. Im nächsten Schritt müssen Sie den Dienst weiterhin ausführen, während Sie das Konsolen Anwendungsprojekt ändern.

## <a name="generate-the-service-proxy"></a>Generieren des Dienst Proxys

Der Dienst Proxy ist eine .NET-Klasse, die Methoden für den Zugriff auf den odata-Dienst definiert. Der Proxy übersetzt Methodenaufrufe in HTTP-Anforderungen. Sie erstellen die Proxy Klasse, indem Sie eine [T4-Vorlage](https://msdn.microsoft.com/library/bb126445.aspx)ausführen.

Klicken Sie mit der rechten Maustaste auf das Projekt. Wählen Sie **Hinzufügen** &gt; **Neues Element** aus.

![](create-an-odata-v4-client-app/_static/image5.png)

Wählen Sie im Dialogfeld **Neues Element hinzufügen** die Option  **C# visuelle Elemente** &gt; **Code** &gt; **odata-Client**aus. Benennen Sie die Vorlage &quot;ProductClient.tt&quot;. Klicken Sie auf **Hinzufügen** , und klicken Sie auf die Sicherheitswarnung.

[![](create-an-odata-v4-client-app/_static/image7.png)](create-an-odata-v4-client-app/_static/image6.png)

An diesem Punkt erhalten Sie eine Fehlermeldung, die Sie ignorieren können. Visual Studio führt die Vorlage automatisch aus, aber die Vorlage benötigt zunächst einige Konfigurationseinstellungen.

[![](create-an-odata-v4-client-app/_static/image9.png)](create-an-odata-v4-client-app/_static/image8.png)

Öffnen Sie die Datei "productclient. odata. config". Fügen Sie im `Parameter`-Element den URI aus dem productservice-Projekt (vorheriger Schritt) ein. Beispiel:

[!code-xml[Main](create-an-odata-v4-client-app/samples/sample1.xml)]

[![](create-an-odata-v4-client-app/_static/image11.png)](create-an-odata-v4-client-app/_static/image10.png)

Führen Sie die Vorlage erneut aus. Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf die Datei ProductClient.tt, und wählen Sie **benutzerdefiniertes Tool**

Die Vorlage erstellt eine Codedatei mit dem Namen ProductClient.cs, die den Proxy definiert. Wenn Sie beim Entwickeln der APP den odata-Endpunkt ändern, führen Sie die Vorlage erneut aus, um den Proxy zu aktualisieren.

![](create-an-odata-v4-client-app/_static/image12.png)

## <a name="use-the-service-proxy-to-call-the-odata-service"></a>Verwenden des Dienst Proxys, um den odata-Dienst aufzurufen

Öffnen Sie die Datei Program.cs, und ersetzen Sie den Code Bausteine durch Folgendes.

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample2.cs)]

Ersetzen Sie den Wert von *ServiceUri* durch den Dienst-URI von früheren Versionen.

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample3.cs)]

Wenn Sie die app ausführen, sollte Sie Folgendes ausgeben:

[!code-console[Main](create-an-odata-v4-client-app/samples/sample4.cmd)]
