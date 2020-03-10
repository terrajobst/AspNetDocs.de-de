---
uid: web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
title: Entity Framework, wenn Komponententests ASP.net-Web-API 2 | Microsoft-Dokumentation
author: Rick-Anderson
description: Diese Anleitung und Anwendung veranschaulichen, wie Sie Komponententests für Ihre Web-API 2-Anwendung erstellen, die die Entity Framework verwendet. Es wird gezeigt, wie Sie das ändern...
ms.author: riande
ms.date: 12/13/2013
ms.assetid: cd844025-ccad-41ce-8694-595f1022a49f
msc.legacyurl: /web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 258450107ee7443c4efd43a3b8e4851249745227
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78447087"
---
# <a name="mocking-entity-framework-when-unit-testing-aspnet-web-api-2"></a>Entity Framework, wenn Komponententests ASP.net-Web-API 2

von [Tom fitzmacken](https://github.com/tfitzmac)

[Herunterladen des abgeschlossenen Projekts](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)

> Diese Anleitung und Anwendung veranschaulichen, wie Sie Komponententests für Ihre Web-API 2-Anwendung erstellen, die die Entity Framework verwendet. Es zeigt, wie der Gerüst Controller geändert wird, um das Übergeben eines Kontext Objekts für Tests zu ermöglichen, und wie Testobjekte erstellt werden, die mit Entity Framework funktionieren.
>
> Eine Einführung in Komponententests mit ASP.net-Web-API finden Sie unter Komponenten [Tests mit ASP.net-Web-API 2](unit-testing-with-aspnet-web-api.md).
>
> In diesem Tutorial wird davon ausgegangen, dass Sie mit den grundlegenden Konzepten von ASP.net-Web-API vertraut sind. Ein Einführ Endes Tutorial finden Sie unter [Getting Started with ASP.net-Web-API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).
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
- [Erstellen der Modell Klasse](#modelclass)
- [Hinzufügen des Controllers](#controller)
- [Abhängigkeitsinjektion hinzufügen](#dependency)
- [Installieren von nuget-Paketen im Testprojekt](#testpackages)
- [Test Kontext erstellen](#testcontext)
- [Erstellen von Tests](#tests)
- [Tests ausführen](#runtests)

Wenn Sie die Schritte in Komponenten [Tests mit ASP.net-Web-API 2](unit-testing-with-aspnet-web-api.md)bereits ausgeführt haben, können Sie zum Abschnitt [Hinzufügen des Controllers](#controller)wechseln.

<a id="prereqs"></a>
## <a name="prerequisites"></a>Voraussetzungen

Visual Studio 2017 Community, Professional oder Enterprise Edition

<a id="download"></a>
## <a name="download-code"></a>Code herunterladen

Laden Sie das [abgeschlossene Projekt](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)herunter. Das herunterladbare Projekt enthält Komponenten Testcode für dieses Thema und für das Thema Komponenten [Tests ASP.net-Web-API 2](unit-testing-with-aspnet-web-api.md) .

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a>Erstellen einer Anwendung mit einem Komponenten Testprojekt

Sie können entweder ein Komponenten Testprojekt erstellen, wenn Sie die Anwendung erstellen oder einer vorhandenen Anwendung ein Komponenten Testprojekt hinzufügen. Dieses Tutorial zeigt, wie Sie ein Komponenten Testprojekt erstellen, wenn Sie die Anwendung erstellen.

Erstellen Sie eine neue ASP.NET-Webanwendung mit dem Namen **storeapp**.

Wählen Sie in den neuen ASP.net-Projekt Fenstern die **leere** Vorlage aus, und fügen Sie Ordner und Kern Verweise für die Web-API hinzu. Wählen Sie die Option Komponenten **Tests hinzufügen** aus. Das Komponenten Testprojekt wird automatisch mit dem Namen **storeapp. Tests**benannt. Sie können diesen Namen beibehalten.

![Komponenten Testprojekt erstellen](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image1.png)

Nachdem Sie die Anwendung erstellt haben, werden Sie sehen, dass Sie zwei Projekte enthält: **storeapp** und **storeapp. Tests**.

<a id="modelclass"></a>
## <a name="create-the-model-class"></a>Erstellen der Modell Klasse

Fügen Sie im Projekt storeapp dem Ordner **Models** eine Klassendatei mit dem Namen **Product.cs**hinzu. Ersetzen Sie den Inhalt der Datei durch den folgenden Code.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample1.cs)]

Erstellen Sie die Projektmappe.

<a id="controller"></a>
## <a name="add-the-controller"></a>Hinzufügen des Controllers

Klicken Sie mit der rechten Maustaste auf den Ordner Controller, und wählen Sie **Hinzufügen** und **Neues Gerüst Element**aus. Wählen Sie Web-API 2-Controller mit Aktionen unter Verwendung von Entity Framework aus.

![neuen Controller hinzufügen](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image2.png)

Legen Sie die folgenden Werte fest:

- Controller Name: **ProductController**
- Modell Klasse: **Produkt**
- Datenkontext Klasse: [Wählen Sie eine **neue Datenkontext** Schaltfläche aus, die die unten gezeigten Werte füllt.]

![Controller angeben](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image3.png)

Klicken Sie auf **Hinzufügen** , um den Controller mit automatisch generiertem Code zu erstellen. Der Code enthält Methoden zum Erstellen, abrufen, aktualisieren und Löschen von Instanzen der Product-Klasse. Der folgende Code zeigt die-Methode für das Hinzufügen eines Produkts. Beachten Sie, dass die-Methode eine Instanz von **ihttpactionresult**zurückgibt.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample2.cs)]

Ihttpactionresult ist eines der neuen Features in der Web-API 2 und vereinfacht die Entwicklung von Komponententests.

Im nächsten Abschnitt wird der generierte Code angepasst, um das Übergeben von Testobjekten an den Controller zu vereinfachen.

<a id="dependency"></a>
## <a name="add-dependency-injection"></a>Abhängigkeitsinjektion hinzufügen

Derzeit ist die ProductController-Klasse hart codiert, um eine Instanz der storeappcontext-Klasse zu verwenden. Sie verwenden ein Muster namens Abhängigkeitsinjektion, um die Anwendung zu ändern und diese hart codierte Abhängigkeit zu entfernen. Wenn Sie diese Abhängigkeit unterbrechen, können Sie beim Testen ein Mock-Objekt übergeben.

Klicken Sie mit der rechten Maustaste auf den Ordner **Modelle** , und fügen Sie eine neue Schnittstelle namens **istoreappcontext**hinzu.

Ersetzen Sie den Code durch den folgenden Code.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample3.cs)]

Öffnen Sie die Datei StoreAppContext.cs, und nehmen Sie die folgenden hervorgehobenen Änderungen vor. Beachten Sie die folgenden wichtigen Änderungen:

- Storeappcontext-Klasse implementiert istoreappcontext-Schnittstelle
- Die markasmodified-Methode ist implementiert.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample4.cs?highlight=6,14-17)]

Öffnen Sie die Datei ProductController.cs. Ändern Sie den vorhandenen Code so, dass er dem markierten Code entspricht. Diese Änderungen unterbrechen die Abhängigkeit von storeappcontext und ermöglichen es anderen Klassen, ein anderes Objekt für die Kontext Klasse zu übergeben. Diese Änderung ermöglicht es Ihnen, während der Komponententests einen Test Kontext zu übergeben.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample5.cs?highlight=4,7-12)]

Es gibt eine weitere Änderung, die Sie in ProductController vornehmen müssen. Ersetzen Sie in der **putproduct** -Methode die Zeile, die den Entitäts Status auf Modified festlegt, mithilfe eines Aufrufes der markasmodified-Methode.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample6.cs?highlight=14-15)]

Erstellen Sie die Projektmappe.

Sie sind jetzt bereit, das Testprojekt einzurichten.

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a>Installieren von nuget-Paketen im Testprojekt

Wenn Sie die leere Vorlage zum Erstellen einer Anwendung verwenden, enthält das Komponenten Testprojekt (storeapp. Tests) keine installierten nuget-Pakete. Andere Vorlagen (z. b. die Web-API-Vorlage) enthalten einige nuget-Pakete in das Komponenten Testprojekt. Für dieses Tutorial müssen Sie das Entity Framework-Paket und das Microsoft ASP.net Web-API 2-Kern Paket in das Testprojekt einschließen.

Klicken Sie mit der rechten Maustaste auf das Projekt storeapp. Tests und wählen Sie **nuget-Pakete verwalten**aus. Sie müssen das Projekt storeapp. Tests auswählen, um die Pakete dem Projekt hinzuzufügen.

![Verwalten von Paketen](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image4.png)

Suchen Sie in den Online Paketen nach dem EntityFramework-Paket (Version 6,0 oder höher), und installieren Sie es. Wenn das Paket "EntityFramework" bereits installiert ist, haben Sie möglicherweise das Projekt storeapp anstelle des Projekts storeapp. Tests ausgewählt.

![Entity Framework hinzufügen](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image5.png)

Suchen und installieren Sie Microsoft ASP.net Web-API 2-Kernpaket.

![Installieren des Web-API-Kernpakets](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image6.png)

Schließen Sie das Fenster nuget-Pakete verwalten.

<a id="testcontext"></a>
## <a name="create-test-context"></a>Test Kontext erstellen

Fügen Sie dem Testprojekt eine Klasse mit dem Namen **testdbset** hinzu. Diese Klasse dient als Basisklasse für das Test Dataset. Ersetzen Sie den Code durch den folgenden Code.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample7.cs)]

Fügen Sie dem Testprojekt eine Klasse mit dem Namen **testproductdbset** hinzu, die den folgenden Code enthält.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample8.cs)]

Fügen Sie eine Klasse mit dem Namen **teststoreappcontext** hinzu, und ersetzen Sie den vorhandenen Code durch den folgenden Code.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample9.cs)]

<a id="tests"></a>
## <a name="create-tests"></a>Tests erstellen

Standardmäßig enthält das Testprojekt eine leere Testdatei mit dem Namen **UnitTest1.cs**. Diese Datei zeigt die Attribute, die Sie zum Erstellen von Testmethoden verwenden. In diesem Tutorial können Sie diese Datei löschen, da Sie eine neue Testklasse hinzufügen.

Fügen Sie dem Testprojekt eine Klasse mit dem Namen **testproductcontroller** hinzu. Ersetzen Sie den Code durch den folgenden Code.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample10.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a>Tests durchführen

Sie sind jetzt bereit, die Tests auszuführen. Alle Methoden, die mit dem **TestMethod** -Attribut markiert sind, werden getestet. Führen Sie die Tests über das Menü Element **Test** aus.

![Tests durchführen](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image7.png)

Öffnen Sie das Fenster **Test-Explorer** , und sehen Sie sich die Ergebnisse der Tests an.

![Testergebnisse](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image8.png)
