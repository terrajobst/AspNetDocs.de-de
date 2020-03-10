---
uid: web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
title: Komponententests ASP.net-Web-API 2 | Microsoft-Dokumentation
author: Rick-Anderson
description: Dieser Leitfaden und diese Anwendung veranschaulichen, wie Sie einfache Komponententests für Ihre Web-API 2-Anwendung erstellen. In diesem Tutorial wird gezeigt, wie Sie einen Komponenten Test-proj einschließen...
ms.author: riande
ms.date: 06/05/2014
ms.assetid: bf20f78d-ff91-48be-abd1-88e23dcc70ba
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: f2d60b977475e048a3a74aabff4adc768ee22baf
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78446985"
---
# <a name="unit-testing-aspnet-web-api-2"></a>Komponententests ASP.net-Web-API 2

von [Tom fitzmacken](https://github.com/tfitzmac)

[Herunterladen des abgeschlossenen Projekts](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)

> Dieser Leitfaden und diese Anwendung veranschaulichen, wie Sie einfache Komponententests für Ihre Web-API 2-Anwendung erstellen. In diesem Tutorial wird gezeigt, wie Sie ein Komponenten Testprojekt in die Projekt Mappe einschließen und Testmethoden schreiben, mit denen die zurückgegebenen Werte von einer Controller Methode überprüft werden.
>
> In diesem Tutorial wird davon ausgegangen, dass Sie mit den grundlegenden Konzepten von ASP.net-Web-API vertraut sind. Ein Einführ Endes Tutorial finden Sie unter [Getting Started with ASP.net-Web-API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).
>
> Die Komponententests in diesem Thema sind absichtlich auf einfache Daten Szenarien beschränkt. Weitere Informationen zu Komponententests für erweiterte Daten Szenarios finden Sie unter [Entity Framework bei Unittests ASP.net-Web-API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Im Tutorial verwendete Software Versionen
>
> - [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - Web-API 2

## <a name="in-this-topic"></a>In diesem Thema

Dieses Thema enthält folgende Abschnitte:

- [Erforderliche Komponenten](#prereqs)
- [Code herunterladen](#download)
- [Erstellen einer Anwendung mit einem Komponenten Testprojekt](#appwithunittest)
    - [Komponenten Testprojekt beim Erstellen der Anwendung hinzufügen](#whencreate)
    - [Hinzufügen eines Komponenten Testprojekts zu einer vorhandenen Anwendung](#addtoexisting)
- [Einrichten der Web-API 2-Anwendung](#setupproject)
- [Installieren von nuget-Paketen im Testprojekt](#testpackages)
- [Erstellen von Tests](#tests)
- [Tests ausführen](#runtests)

<a id="prereqs"></a>
## <a name="prerequisites"></a>Voraussetzungen

[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional oder Enterprise Edition

<a id="download"></a>
## <a name="download-code"></a>Code herunterladen

Laden Sie das [abgeschlossene Projekt](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)herunter. Das herunterladbare Projekt enthält Komponenten Testcode für dieses Thema und für den [Entity Framework bei Komponententests ASP.net-Web-API](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) .

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a>Erstellen einer Anwendung mit einem Komponenten Testprojekt

Sie können entweder ein Komponenten Testprojekt erstellen, wenn Sie die Anwendung erstellen oder einer vorhandenen Anwendung ein Komponenten Testprojekt hinzufügen. Dieses Tutorial zeigt beide Methoden zum Erstellen eines Komponenten Testprojekts. Um diesem Tutorial zu folgen, können Sie beide Ansätze verwenden.

<a id="whencreate"></a>
### <a name="add-unit-test-project-when-creating-the-application"></a>Komponenten Testprojekt beim Erstellen der Anwendung hinzufügen

Erstellen Sie eine neue ASP.NET-Webanwendung mit dem Namen **storeapp**.

![Erstellen des Projekts](unit-testing-with-aspnet-web-api/_static/image1.png)

Wählen Sie in den neuen ASP.net-Projekt Fenstern die **leere** Vorlage aus, und fügen Sie Ordner und Kern Verweise für die Web-API hinzu. Wählen Sie die Option Komponenten **Tests hinzufügen** aus. Das Komponenten Testprojekt wird automatisch mit dem Namen **storeapp. Tests**benannt. Sie können diesen Namen beibehalten.

![Komponenten Testprojekt erstellen](unit-testing-with-aspnet-web-api/_static/image2.png)

Nachdem Sie die Anwendung erstellt haben, werden Sie sehen, dass Sie zwei Projekte enthält.

![zwei Projekte](unit-testing-with-aspnet-web-api/_static/image3.png)

<a id="addtoexisting"></a>
### <a name="add-unit-test-project-to-an-existing-application"></a>Hinzufügen eines Komponenten Testprojekts zu einer vorhandenen Anwendung

Wenn Sie das Komponenten Testprojekt nicht erstellt haben, als Sie die Anwendung erstellt haben, können Sie es jederzeit hinzufügen. Nehmen wir beispielsweise an, dass Sie bereits eine Anwendung mit dem Namen storeapp haben und Komponententests hinzufügen möchten. Um ein Komponenten Testprojekt hinzuzufügen, klicken Sie mit der rechten Maustaste auf die Projekt Mappe, und wählen Sie **Hinzufügen** und **Neues Projekt**

![Neues Projekt zur Projekt Mappe hinzufügen](unit-testing-with-aspnet-web-api/_static/image4.png)

Wählen Sie im linken Bereich **Test** aus, und wählen Sie Komponenten **Test Projekt** für den Projekttyp aus. Nennen Sie das Projekt **storeapp. Tests**.

![Komponenten Testprojekt hinzufügen](unit-testing-with-aspnet-web-api/_static/image5.png)

Das Komponenten Testprojekt wird in der Projekt Mappe angezeigt.

Fügen Sie im Komponenten Testprojekt einen Projekt Verweis zum ursprünglichen Projekt hinzu.

<a id="setupproject"></a>
## <a name="set-up-the-web-api-2-application"></a>Einrichten der Web-API 2-Anwendung

Fügen Sie im Projekt storeapp dem Ordner **Models** eine Klassendatei mit dem Namen **Product.cs**hinzu. Ersetzen Sie den Inhalt der Datei durch den folgenden Code.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample1.cs)]

Erstellen Sie die Projektmappe.

Klicken Sie mit der rechten Maustaste auf den Ordner Controller, und wählen Sie **Hinzufügen** und **Neues Gerüst Element**aus. Wählen Sie **Web-API 2-Controller-leer**aus.

![neuen Controller hinzufügen](unit-testing-with-aspnet-web-api/_static/image6.png)

Legen Sie den Controller Namen auf **simpleproductcontroller**fest, und klicken Sie auf **Hinzufügen**.

![Controller angeben](unit-testing-with-aspnet-web-api/_static/image7.png)

Ersetzen Sie den vorhandenen Code durch folgenden Code: Um dieses Beispiel zu vereinfachen, werden die Daten in einer Liste und nicht in einer Datenbank gespeichert. Die in dieser Klasse definierte Liste stellt die Produktionsdaten dar. Beachten Sie, dass der Controller einen Konstruktor enthält, der als Parameter eine Liste von Produkt Objekten annimmt. Dieser Konstruktor ermöglicht Ihnen das Übergeben von Testdaten bei Unittests. Der Controller enthält auch zwei **Async** -Methoden, um Komponententests für asynchrone Methoden zu veranschaulichen. Diese asynchronen Methoden wurden durch Aufrufen von **Task. fromresult** implementiert, um den überflüssigen Code zu minimieren, normalerweise enthalten die Methoden jedoch ressourcenintensive Vorgänge.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample2.cs)]

Die GetProduct-Methode gibt eine Instanz der **ihttpactionresult** -Schnittstelle zurück. Ihttpactionresult ist eines der neuen Features in der Web-API 2 und vereinfacht die Entwicklung von Komponententests. Klassen, die die ihttpactionresult-Schnittstelle implementieren, finden Sie im [System. Web. http. results](https://msdn.microsoft.com/library/system.web.http.results.aspx) -Namespace. Diese Klassen stellen mögliche Antworten aus einer Aktions Anforderung dar und entsprechen HTTP-Statuscodes.

Erstellen Sie die Projektmappe.

Sie sind jetzt bereit, das Testprojekt einzurichten.

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a>Installieren von nuget-Paketen im Testprojekt

Wenn Sie die leere Vorlage zum Erstellen einer Anwendung verwenden, enthält das Komponenten Testprojekt (storeapp. Tests) keine installierten nuget-Pakete. Andere Vorlagen (z. b. die Web-API-Vorlage) enthalten einige nuget-Pakete in das Komponenten Testprojekt. Für dieses Tutorial müssen Sie das Microsoft ASP.net Web-API 2-Kern Paket in das Testprojekt einschließen.

Klicken Sie mit der rechten Maustaste auf das Projekt storeapp. Tests und wählen Sie **nuget-Pakete verwalten**aus. Sie müssen das Projekt storeapp. Tests auswählen, um die Pakete dem Projekt hinzuzufügen.

![Verwalten von Paketen](unit-testing-with-aspnet-web-api/_static/image8.png)

Suchen und installieren Sie Microsoft ASP.net Web-API 2-Kernpaket.

![Installieren des Web-API-Kernpakets](unit-testing-with-aspnet-web-api/_static/image9.png)

Schließen Sie das Fenster nuget-Pakete verwalten.

<a id="tests"></a>
## <a name="create-tests"></a>Tests erstellen

Standardmäßig enthält das Testprojekt eine leere Testdatei mit dem Namen UnitTest1.cs. Diese Datei zeigt die Attribute, die Sie zum Erstellen von Testmethoden verwenden. Für die Komponententests können Sie diese Datei verwenden oder eine eigene Datei erstellen.

![UnitTest1](unit-testing-with-aspnet-web-api/_static/image10.png)

In diesem Tutorial erstellen Sie eine eigene Testklasse. Sie können die Datei UnitTest1.cs löschen. Fügen Sie eine Klasse mit dem Namen **TestSimpleProductController.cs**hinzu, und ersetzen Sie den Code durch den folgenden Code.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample3.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a>Tests durchführen

Sie sind jetzt bereit, die Tests auszuführen. Alle Methoden, die mit dem **TestMethod** -Attribut markiert sind, werden getestet. Führen Sie die Tests über das Menü Element **Test** aus.

![Tests durchführen](unit-testing-with-aspnet-web-api/_static/image11.png)

Öffnen Sie das Fenster **Test-Explorer** , und sehen Sie sich die Ergebnisse der Tests an.

![Testergebnisse](unit-testing-with-aspnet-web-api/_static/image12.png)

## <a name="summary"></a>Zusammenfassung

Sie haben dieses Lernprogramm abgeschlossen. Die Daten in diesem Tutorial wurden absichtlich vereinfacht, um sich auf Komponenten Testbedingungen zu konzentrieren. Weitere Informationen zu Komponententests für erweiterte Daten Szenarios finden Sie unter [Entity Framework bei Unittests ASP.net-Web-API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).
